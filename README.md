
Hash Cons'ing for Rust
======================

- A `Merkle<T>` gives a _compact serialization_ in the presence of
  sharing, i.e., when many structures share a common instance of a
  `T`.

- A `Hc<T>` gives a _unique representation_ in the presence of
  sharing, i.e., when many structures share a common instance of a
  `T`.

Status
---------

- The type `Merkle<_>` is implemented and tested
- The type `Hc<_>` is a minor variation; it remains as future work

Background
-----------

Sometimes, we want a shared instance of some type `T` that serializes
*once*, not _once per reference_, as is the case with the `Rc` type.

Unlike a "bare" `Rc<T>`, a `Merkle<T>` enjoys the practical property
that, when a structure holding multiple (shared) instances of `Merkle<T>` is
serialized, this serialized output holds only _one_ occurrence of
each `T`'s serialized representation; the other occurrences merely
consist of the `T`'s unique identifier (the serialization of an
`Id`, single machine word on modern machines).

Implementation summary
-----------------------

A `Merkle<T>` has a unique ID (computed as a hash) that permits table-based indirection,
via temporary storage used by serialization and serialization logic.

By contrast, a bare `Rc<T>` lacks this indirection, and thus, it lacks
a compact serialized representation for structures with abundant
sharing. Generally, abundant sharing via many shared `Rc<_>`s leads to
*exponential blow up* in terms of serialized space and time.


