---
layout: post
title: Linux kernel의 Radix-tree와 Xarray
image: linuxfoundation.jpg
author: Jaeyoun
date: 2020-03-11T10:03:47.149Z
tags: 
  - linux
---

Linux 커널 내부의 Radix-tree 코드를 분석해보았다.
> 커널버전 : 5.3

---

# Radix tree
Radix tree는 리눅스 커널 내에서 page cache등을 관리하기위해 사용된 자료구조이다.

# Xarray
Xarray는 Radix tree의 성능을 개선하여 나온 자료구조이다. 4.19버전 이후로 merge된 것으로 보인다.

아래 Radix tree 코드를 보면 #define으로 xarray로 치환됨을 볼 수 있다.


# Radix tree 테스팅

```tools/testing/radix-tree```에 테스트코드가 존재한다.

```make```명령을 수행하면 urcu가 없다고 에러가 뜰것인데, [이곳](https://liburcu.org/)에서 다운로드 받을 수 있다.

```
$ git clone git://git.liburcu.org/userspace-rcu.git
$ cd userspace-rcu/
$ ./configure
$ make
$ sudo make install
$ sudo ldconfig
```

그 후에 ```tools/testing/radix-tree```에서 ```make```를 수행하면 빌드가 된다.

여러 실행파일 들이 생긴 것을 확인할 수 있다. (main, xarray, ...)

main함수를 실행해보면 테스트를 진행하고 종료한다.

```./main -v``` -v 옵션을 주면 더 많은 정보를 출력한다.
```./main -vv```는 위보다도 더 많은 정보를 출력한다.

---

# Radix Tree
Radix Tree의 소스코드는 ```lib/radix-tree.c```에 있다.

## Insert
Radix tree insert코드는 아래와 같다.

```
/**                                                                                                                                                                                   
 *  __radix_tree_insert    -    insert into a radix tree                                                                                                                              
 *  @root:      radix tree root                                                                                                                                                       
 *  @index:     index key                                                                                                                                                             
 *  @item:      item to insert                                                                                                                                                        
 *                                                                                                                                                                                    
 *  Insert an item into the radix tree at position @index.                                                                                                                            
 */                                                                                                                                                                                   
int radix_tree_insert(struct radix_tree_root *root, unsigned long index,                                                                                                              
            void *item)                                                                                                                                                               
{                                                                                                                                                                                     
    struct radix_tree_node *node;                                                                                                                                                     
    void __rcu **slot;                                                                                                                                                                
    int error;                                                                                                                                                                        
                                                                                                                                                                                      
    BUG_ON(radix_tree_is_internal_node(item));                                                                                                                                        
                                                                                                                                                                                      
    error = __radix_tree_create(root, index, &node, &slot);                                                                                                                           
    if (error)                                                                                                                                                                        
        return error;                                                                                                                                                                 
                                                                                                                                                                                      
    error = insert_entries(node, slot, item, false);                                                                                                                                  
    if (error < 0)                                                                                                                                                                    
        return error;                                                                                                                                                                 
                                                                                                                                                                                      
    if (node) {                                                                                                                                                                       
        unsigned offset = get_slot_offset(node, slot);                                                                                                                                
        BUG_ON(tag_get(node, 0, offset));                                                                                                                                             
        BUG_ON(tag_get(node, 1, offset));                                                                                                                                             
        BUG_ON(tag_get(node, 2, offset));                                                                                                                                             
    } else {                                                                                                                                                                          
        BUG_ON(root_tags_get(root));                                                                                                                                                  
    }                                                                                                                                                                                 
                                                                                                                                                                                      
    return 0;                                                                                                                                                                         
}                                                                                                                                                                                     
EXPORT_SYMBOL(radix_tree_insert);
```

## Create

```
/**                       
 *  __radix_tree_create -   create a slot in a radix tree
 *  @root:      radix tree root
 *  @index:     index key
 *  @nodep:     returns node
 *  @slotp:     returns slot
 *                                        
 *  Create, if necessary, and return the node and slot for an item
 *  at position @index in the radix tree @root.
 *                                                                        
 *  Until there is more than one item in the tree, no nodes are            
 *  allocated and @root->xa_head is used as a direct slot instead of
 *  pointing to a node, in which case *@nodep will be NULL.
 *
 *  Returns -ENOMEM, or 0 for success.                                      
 */                                                                            
static int __radix_tree_create(struct radix_tree_root *root,
        unsigned long index, struct radix_tree_node **nodep,
        void __rcu ***slotp)
{
    struct radix_tree_node *node = NULL, *child;
    void __rcu **slot = (void __rcu **)&root->xa_head;
    unsigned long maxindex;
    unsigned int shift, offset = 0;
    unsigned long max = index;
    gfp_t gfp = root_gfp_mask(root);

    shift = radix_tree_load_root(root, &child, &maxindex);
                                                                                                                                                                                                                     
    /* Make sure the tree is high enough.  */
    if (max > maxindex) {
        int error = radix_tree_extend(root, gfp, max, shift);        
        if (error < 0)
            return error;                                                  
        shift = error;                                      
        child = rcu_dereference_raw(root->xa_head);
    }             

        while (shift > 0) {
        shift -= RADIX_TREE_MAP_SHIFT;
        if (child == NULL) {
            /* Have to add a child node.  */
            child = radix_tree_node_alloc(gfp, node, root, shift,
                            offset, 0, 0);               
            if (!child)
                return -ENOMEM;
            rcu_assign_pointer(*slot, node_to_entry(child));
            if (node)
                node->count++;
        } else if (!radix_tree_is_internal_node(child))
            break;

        /* Go a level down */
        node = entry_to_node(child);
        offset = radix_tree_descend(node, &child, index);
        slot = &node->slots[offset];
    }

    if (nodep)
        *nodep = node;
    if (slotp)
        *slotp = slot;
    return 0;
}

```


## Extend
저장할 수 없는 범위의 index를 넘어가면 해당 인덱스를 저장 할 수 있도록 radix tree를 확장한다.

```
/*
 *  Extend a radix tree so it can store key @index.
 */
static int radix_tree_extend(struct radix_tree_root *root, gfp_t gfp,
                unsigned long index, unsigned int shift)                                                                                                                              
{                                                                                                                                                                                     
    void *entry;
    unsigned int maxshift;
    int tag;                        

    /* Figure out what the shift should be.  */
    maxshift = shift;
    while (index > shift_maxindex(maxshift))
        maxshift += RADIX_TREE_MAP_SHIFT;

    entry = rcu_dereference_raw(root->xa_head);
    if (!entry && (!is_idr(root) || root_tag_get(root, IDR_FREE)))
        goto out;

    do {
        struct radix_tree_node *node = radix_tree_node_alloc(gfp, NULL,
                            root, shift, 0, 1, 0);                   
        if (!node)                                                    
            return -ENOMEM;                                         

        if (is_idr(root)) {                                           
            all_tag_set(node, IDR_FREE);                              
            if (!root_tag_get(root, IDR_FREE)) {    
                tag_clear(node, IDR_FREE, 0);
                root_tag_set(root, IDR_FREE);                  
            }
        } else {
            /* Propagate the aggregated tag info to the new child */
            for (tag = 0; tag < RADIX_TREE_MAX_TAGS; tag++) {
                if (root_tag_get(root, tag))
                    tag_set(node, tag, 0);
            }
        }

        BUG_ON(shift > BITS_PER_LONG);
        if (radix_tree_is_internal_node(entry)) {
            entry_to_node(entry)->parent = node;
        } else if (xa_is_value(entry)) {
            /* Moving a value entry root->xa_head to a node */
            node->nr_values = 1;
        }

        /*
         * entry was already in the radix tree, so we do not need
         * rcu_assign_pointer here
         */
        node->slots[0] = (void __rcu *)entry;
        entry = node_to_entry(node);
        rcu_assign_pointer(root->xa_head, entry);
        shift += RADIX_TREE_MAP_SHIFT;
    } while (shift <= maxshift);
out:
    return maxshift + RADIX_TREE_MAP_SHIFT;
}

```


# Radix Tree 구조체
```include/linux/radix-tree.h```에 있다.
Xarray로 대체된 것을 확인할 수 있다.

```
/* Keep unconverted code working */
#define radix_tree_root     xarray
#define radix_tree_node     xa_node
```

```include/linux/xarray.h```에 해당 구조체 선언을 확인 할 수 있다.

```
/**
 * struct xarray - The anchor of the XArray.
 * @xa_lock: Lock that protects the contents of the XArray.
 *
 * To use the xarray, define it statically or embed it in your data structure.
 * It is a very small data structure, so it does not usually make sense to
 * allocate it separately and keep a pointer to it in your data structure.
 *
 * You may use the xa_lock to protect your own data structures as well.
 */
/*
 * If all of the entries in the array are NULL, @xa_head is a NULL pointer.
 * If the only non-NULL entry in the array is at index 0, @xa_head is that
 * entry.  If any other entry in the array is non-NULL, @xa_head points
 * to an @xa_node.
 */
struct xarray {
    spinlock_t  xa_lock;
/* private: The rest of the data structure is not to be used directly. */
    gfp_t       xa_flags;
    void __rcu *    xa_head;
};
```

```
/*
 * @count is the count of every non-NULL element in the ->slots array
 * whether that is a value entry, a retry entry, a user pointer,
 * a sibling entry or a pointer to the next level of the tree.
 * @nr_values is the count of every element in ->slots which is
 * either a value entry or a sibling of a value entry.
 */
struct xa_node {                                                                                                                                                                                                     
    unsigned char   shift;      /* Bits remaining in each slot */
    unsigned char   offset;     /* Slot offset in parent */
    unsigned char   count;      /* Total entry count */
    unsigned char   nr_values;  /* Value entry count */
    struct xa_node __rcu *parent;   /* NULL at top of tree */
    struct xarray   *array;     /* The array we belong to */
    union {
        struct list_head private_list;  /* For tree user */
        struct rcu_head rcu_head;   /* Used when freeing node */
    };
    void __rcu  *slots[XA_CHUNK_SIZE];
    union {
        unsigned long   tags[XA_MAX_MARKS][XA_MARK_LONGS];
        unsigned long   marks[XA_MAX_MARKS][XA_MARK_LONGS];
    }; 
};
```

radix tree의 insert에 해당하는 Xarray의 insert, 내부적으로 xas_store함수를 부른다.

```
/**
 * __xa_insert() - Store this entry in the XArray if no entry is present.
 * @xa: XArray.
 * @index: Index into array.
 * @entry: New entry.
 * @gfp: Memory allocation flags.
 *
 * Inserting a NULL entry will store a reserved entry (like xa_reserve())
 * if no entry is present.  Inserting will fail if a reserved entry is
 * present, even though loading from this index will return NULL.
 *
 * Context: Any context.  Expects xa_lock to be held on entry.  May
 * release and reacquire xa_lock if @gfp flags permit.
 * Return: 0 if the store succeeded.  -EBUSY if another entry was present.
 * -ENOMEM if memory could not be allocated.
 */
int __xa_insert(struct xarray *xa, unsigned long index, void *entry, gfp_t gfp) 
{ 
    XA_STATE(xas, xa, index); 
    void *curr; 
 
    if (WARN_ON_ONCE(xa_is_advanced(entry))) 
        return -EINVAL; 
    if (!entry) 
        entry = XA_ZERO_ENTRY; 
 
    do { 
        curr = xas_load(&xas); 
        if (!curr) { 
            xas_store(&xas, entry); 
            if (xa_track_free(xa)) 
                xas_clear_mark(&xas, XA_FREE_MARK); 
        } else {
            xas_set_err(&xas, -EBUSY);
        }
    } while (__xas_nomem(&xas, gfp));

    return xas_error(&xas);
}
EXPORT_SYMBOL(__xa_insert);
```

```
/**                                                                                                                                                                                   
 * __xa_store() - Store this entry in the XArray.                                                                                                                                     
 * @xa: XArray.                                                                                                                                                                       
 * @index: Index into array.                                                                                                                                                          
 * @entry: New entry.                                                                                                                                                                 
 * @gfp: Memory allocation flags.                                                                                                                                                     
 *                                                                                                                                                                                    
 * You must already be holding the xa_lock when calling this function.                                                                                                                
 * It will drop the lock if needed to allocate memory, and then reacquire                                                                                                             
 * it afterwards.                                                                                                                                                                     
 *                                                                                                                                                                                    
 * Context: Any context.  Expects xa_lock to be held on entry.  May                                                                                                                   
 * release and reacquire xa_lock if @gfp flags permit.                                                                                                                                
 * Return: The old entry at this index or xa_err() if an error happened.                                                                                                              
 */                                                                                                                                                                                   
void *__xa_store(struct xarray *xa, unsigned long index, void *entry, gfp_t gfp)                                                                                                      
{                                                                                                                                                                                     
    XA_STATE(xas, xa, index);                                                                                                                                                         
    void *curr;                                                                                                                                                                       
                                                                                                                                                                                      
    if (WARN_ON_ONCE(xa_is_advanced(entry)))                                                                                                                                          
        return XA_ERROR(-EINVAL);                                                                                                                                                     
    if (xa_track_free(xa) && !entry)                                                                                                                                                  
        entry = XA_ZERO_ENTRY;                                                                                                                                                        
                                                                                                                                                                                      
    do {                                                                                                                                                                              
        curr = xas_store(&xas, entry);                                                                                                                                                
        if (xa_track_free(xa))                                                                                                                                                        
            xas_clear_mark(&xas, XA_FREE_MARK);                                                                                                                                       
    } while (__xas_nomem(&xas, gfp));                                                                                                                                                 
                                                                                                                                                                                      
    return xas_result(&xas, curr);                                                                                                                                                                                   
}                                                                                                                                                                                     
EXPORT_SYMBOL(__xa_store);
```

