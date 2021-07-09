# The plugin system

*The Machinery* is built around a plugin model. All features, even the built-in ones, are provided
through plugins. You can extend *The Machinery* by writing your own plugins.

When *The Machinery* launches, it loads all the plugins named `tm_*.dll` in its `plugins/` folder. If
you write your own plugins, name them so that they start with `tm_` and put them in this folder,
they will be loaded together with the built-in plugins.

*The Machinery* is organized into individual APIs that can be called to perform specific tasks. A
plugin is a DLL that exposes one or several of these APIs. In order to implement its functionality,
the plugin may in turn rely on APIs exposed by other plugins.

A central object called the [tm_api_registry_api](https://ourmachinery.com//apidoc/foundation/api_registry.h.html#structtm_api_registry_api) is used to keep track of all the APIs. When you want to use an API from another plugin, you ask the API registry for it. Similarly, you expose your APIs to the world by registering them with the
[tm_api_registry_api](https://ourmachinery.com//apidoc/foundation/api_registry.h.html#structtm_api_registry_api).

This may seem a bit abstract at this point, so let’s look at a concrete example, [unicode.h](https://ourmachinery.com//apidoc/foundation/unicode.h.html#unicode.h) which exposes the
[tm_unicode_api](https://ourmachinery.com//apidoc/foundation/unicode.h.html#structtm_unicode_api) for encoding an d decoding Unicode strings:

~~~c
// This header has been abridged and commented from the original to make things
// clearer.

// Include standard header.
#include "api_types.h"

// Forward declarations.
struct tm_temp_allocator_i;

// Unicode helper functions.
struct tm_unicode_api
{
    // Encodes the `codepoint` as UTF-8 into `utf8` and returns a pointer to the
    // position where to insert the next codepoint. `utf8` should have room for at
    // least four bytes (the maximum size of a UTF-8 encoded codepoint).
    uint8_t *(*utf8_encode)(uint8_t *utf8, uint32_t codepoint);

    // Decodes and returns the first codepoint in the UTF-8 string `utf8`. The string
    // pointer is advanced to point to the next codepoint in the string. Will generate
    // an error message if the string is not a UTF-8 string.
    uint32_t (*utf8_decode)(const uint8_t **utf8);

    // ...
};

// Name of this API in the registry.
#define TM_UNICODE_API_NAME "tm_unicode_api"
~~~

Let’s go through this.

First, the code includes [api_types.h](https://ourmachinery.com//apidoc/foundation/api_types.h.html#api_types.h). This is a shared header with common type declarations. It includes things like `<stdbool.h>` and `<stdint.h>` and also defines a few *The Machinery* specific types, such as [tm_vec3_t](file:///C:/work/themachinery/build/doc/foundation/api_types.h.html#structtm_vec3_t).

In *The Machinery* we have a rule that header files can't include other header files (except for
[api_types.h](https://ourmachinery.com//apidoc/foundation/api_types.h.html#api_types.h)). This helps keep compile times down, but it also simplifies the structure of the code. When you read a header file you don’t have to follow a long chain of other header files to understand what is happening.

Next follows a block of forward struct declarations (in this case only one).

Next we have the [struct tm_unicode_api](https://ourmachinery.com//apidoc/foundation/unicode.h.html#structtm_unicode_api)  that defines the API, followed by the [TM_UNICODE_API_NAME](https://ourmachinery.com//apidoc/foundation/unicode.h.html#tm_unicode_api_name), the name of the API that other parts of the code can use to query for this API.

To use this API, you would first use the [tm_api_registry_api](https://ourmachinery.com//apidoc/foundation/api_registry.h.html#structtm_api_registry_api) to query for the API pointer, then using that pointer, call the functions of the API:

~~~c
struct tm_unicode_api *tm_unicode_api =
    (struct tm_unicode_api *)tm_global_api_registry->get(TM_UNICODE_API_NAME);

tm_unicode_api->utf8_encode(utf8, codepoint);
~~~

The different APIs that you can query for and use are documented in their respective header files, and in the [apidoc](https://ourmachinery.com/apidoc/apidoc.html) (which is just extracted from the headers). Consult these files for information on how to use the various APIs that are available in *The Machinery.*

In addition to APIs defined in header files, *The Machinery* also contains some header files with inline functions that you can include directly into your implementation files. For example [math.inl](https://ourmachinery.com//apidoc/foundation/math.inl.html#math.inl) provides common
mathematical operations on vectors and matrices, while [carray.inl](https://ourmachinery.com//apidoc/foundation/carray.inl.html#carray.inl) provides a “stretchy-buffer” implementation (i.e. a C version of C++’s `std::vector`).

## Per project plugins

The Engine supports per project plugins through *Plugin Assets*. A Plugin Asset is an asset in the
Asset Browser that holds a plugin.

To create a plugin asset in a particular project, just drop the `.dll` into the Asset Browser
(or use **Import...**). Every time you open the project, the plugin will be loaded.

> **Note**: For security reasons, if you open a project containing plugins, you will be asked
> whether you want to allow the plugins to run or not. Plugins aren't sandboxed, they have full
> access to your machine. So you should only allow project plugins to run if you trust the author
> of the plugin.

When selecting a plugin asset in the asset browser the properties tab will show the following:

![](https://ourmachinery.com/images/tutorials/plugin__properties.png)

If you check the ☒ **Import when changed** checkbox, The Machinery will re-import and reload the
plugin every time it detects a change. You can use this to hot-reload your project plugins.

> **Note:** since The Machinery APIs change with each release version, a plugin DLL built for one
> specific version is unlikely to work with another version. Thus, to open a project with plugin
> DLLs, you should make sure that your version matches the version the DLL was built for. In the
> future, when The Machinery is out of beta, we will provide more stable APIs that will work across
> multiple releases.