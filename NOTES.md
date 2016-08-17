# Reading Notes on [Cuckoo-Filters](https://www.cs.cmu.edu/~dga/papers/cuckoo-conext2014.pdf)

_All algorithms copied from [the original paper](https://www.cs.cmu.edu/~dga/papers/cuckoo-conext2014.pdf)._

These are notes written down to help while implementing the filter.

Maximum number of displacements: ~500 (implement an option)

Implement *partial-key cuckoo hashing* as a seperate package.

## Algorithm 1: Insert(x)

```
f = fingerprint(x);
i1 = hash(x);
i2 = i1 ⊕ hash(f);
if bucket[i1] or bucket[i2] has an empty entry then
  add f to that bucket;
  return Done;

// must relocate existing items;
i = randomly pick i1 or i2;
for n = 0; n < MaxNumKicks; n++ do
  randomly select an entry e from bucket[i];
  swap f and the fingerprint stored in entry e;
  i = i ⊕ hash( f );
  if bucket[i] has an empty entry then
    add f to bucket[i];
    return Done;

// Hashtable is considered full;
return Failure;
```

## Algorithm 2: Lookup(x)

```
f = fingerprint(x);
i1 = hash(x);
i2 = i1 ⊕ hash(f);
if bucket[i1] or bucket[i2] has f then
  return True;

return False;
```

## Algorithm 3: Delete(x)

```
f = fingerprint(x);
i1 = hash(x);
i2 = i1 ⊕ hash(f);
if bucket[i1] or bucket[i2] has f then
  remove a copy of f from this bucket;
  return True;

return False;
```

## API

* Uses currying, for easier usage with Promises or Observables
* Doesn't use `this` for obvious reasons (it would make currying weird/impossible)

```js
const myFilter = cuckooFilter({
  displacements: 500,
  bucketSize: 8,
})
myFilter.add(myFilter)('Some Item')
```

