# payload-interop

The `payload-interop` project defines an interoperable interface for reading
the domain *result* and domain *status* from a _Domain Payload Object_.

## Purpose

The _Domain Payload Object_ pattern was first described by Vaughn Vernon at
[https://vaughnvernon.co/?page_id=40](https://web.archive.org/web/20170620063845/https://vaughnvernon.co/?page_id=40)

Whereas a _Data Transfer Object_ provides *properties* found on domain objects,
a _Domain Payload Object_ provides *entire domain objects*. Its purpose is to
transport those domain objects across the architectural boundary from the domain
layer to the user interface layer.

> **Note:**
>
> The _Domain Payload Object_ is **not** for intra-domain use. It is only for
> transporting a domain result across the domain layer boundary back to the
> calling code, typically the user interface layer.

This project defines only a *reading* interface, so that any user interface code
can know how to get both the result and the status out of the _Domain Payload
Object_.

It does not define a *writing* or *mutation* interface. Creation and
manipulation of _Domain Payload Objects_ are core application concerns, not user
interface concerns. The domain-specific nature places it outside the scope of an
interoperability specification.

## Summary

Research on [Packagist](http://packagist.org) and elsewhere revealed a number
of existing [_Domain Payload Object_ implmentations](./IMPLEMENTATIONS.md).
The commonalities discovered by this research are codified in the
[`DomainPayload` interface](./src/DomainPayload.php) and the
[`DomainStatus` class of constants](./src/DomainStatus.php).

### Domain Result

All of the reference implementations provided functionality to get the domain
results or domain data for presentation. It was noted that these results
included not only domain objects, but also th input as received by the domain,
error or validation messages, exception objects, and so on.

As such, this project expands the semantics of the _Domain Payload Object_
definition to include include any data produced by the domain layer that might
be needed for presentation by the user interface.

This commonality is reflected in the `DomainPayload::getResult() : array`
method.

Having found that commonality, this project encourages implementors to provide
additional reading methods typhinted to their respective domain objects, to
enable better static analysis and automated refactoring.

### Domain Status

The reference implementations also include functionality to discover the
"status" of the attempted domain activity. That is, whether the activity was
successful or not, and what kind of success or failure was encountered.

There was a wide range of status values, occasionally integers but more
frequently strings, defined as constants. Some of these values obviously
mimicked HTTP user interface response codes. Less often, statuses were indicated
by separate classes, rather than by a constant value.

After normalizing and collating the different statuses across the reference
implementations, it turned out there were several that were both user-interface
agnostic and common to most or all implementations. These were most frequently
string valued constants.

This commonality is reflected in the `DomainPayload::getStatus() : string`
method, along with the `DomainStatus` class of constants with these values:

 - `ACCEPTED`
 - `CREATED`
 - `DELETED`
 - `ERROR`
 - `FOUND`
 - `INVALID`
 - `NOT_FOUND`
 - `PROCESSING`
 - `SUCCESS`
 - `UNAUTHORIZED`
 - `UPDATED`
