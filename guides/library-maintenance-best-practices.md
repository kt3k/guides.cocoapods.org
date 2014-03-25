This guide gathers best practices for Cocoa library authors. The goal is to make integration of libraries easier for everyone, using CocoaPods or not. Ultimately, [CocoaPods will leverage](https://github.com/CocoaPods/CocoaPods/issues/1024) existing Xcode projects to build libraries, so this will require some rigor from library authors.

## Pods Discoverability Best Practices

### Podspec

Library authors take a lot of care to develop their libraries, this list of practices helps to ensure that they get the maximum of their effort.

#### Homepage

Set the homepage of the project, the one including the readme, as the the `homepage` of the Pod.


Some authors set their homepage for promotional purposes, however this practice is counterproductive because it loses the context of the tab opened and the user might not recall why it was open or it will force him to navigate a list of libraries to find the one searched and locate the readme. There is nothing wrong with self promotion, and is one to the most important reasons why libraries are released as OSS, the recommendation is to just do it in the readme.

#### Screenshots

Always include screenshots for graphic libraries.

#### Social media URL

If you are interested in feedback or in self promotion use it!

#### License

Always include a license in a dedicated file. The usage of the with curse words might be funny but might hinder the adoption of your library in production applications.

#### Summary & Description

It is important to keep in mind that the summary and the descriptions are used for discovery purposes.

### Implementation

#### Quality of the implementation

There are important metrics which users taken into consideration when deciding among different libraries providing similar features. Some examples of those features are:

- Completeness of the README
- Presence of screenshots for graphic libraries
- Number of users which expressed that they like the library
- Number of issues which are open and number of issues which have been addressed
- Share of public methods which are commented (including parameters and return values)


#### Headers

A pod should always provide a global header named after the Pod:

```
#import <MyDep/MyDep.h>
```
Not only in the majority of the cases the users might be interested to just import all the functionality of the library without fine selections of the headers, but it is also very important in the experimentation phase when a user is just importing a library to your project to test it.


#### Dependencies Headers

Always namespace properly the headers of the dependencies. For example:

```
#import "MyDep.h"
```

```
#import <MyDep/MyDep.h>
```

This avoids confusions and clashes with other libraries

#### Categories

Favour class methods which make obvious the source of a method. If there is interest in providing categories for methods which are called frequently, an optional subspec is recommended.

For example with the following piece of code is not clear where the implementation is coming from:

```
NSString *emoji = [@":smile:" stringByReplacingEmojiAliases];
``` 

Instead with the following one is much clear what is the source of the implementation:

```
NSString *emoji = [IRFEmojiCheatSheet stringByReplacingEmojiAliasesInString: @":smile:"]
```

Definitely a bit longer, but in this usage case, as it is usually the case, it is unlikely that the method will be called often in the final target. Therefore, the extra verbosity is not worth the loss of clarity and the risk of clash with methods which might be introduced by other libraries or to the standard one.

#### Warnings

Pods should compile without errors and warnings out of the box and users should be encouraged to fix them when they encounter any.

### What should be the structure of a library project?

* Xcode workspace
	* Library project
		* Source Code (3 letters prefix)
		* Headers (public/project/private, Copy Files vs Copy Headers)
		* Resources
		* Frameworks (indicative)
		* `DYLIB_COMPATIBILITY_VERSION` (semver)
	* App project
		* `HEADER_SEARCH_PATHS` + `#import <Lib/Lib.h>`

* Separate Demo app project
* Implicit Dependencies
* Multiple (dependent) libraries
* License
* Difference between `#import` in source files and header files
* Unit tests

### Resource bundle

* Info.plist (version information)
* http://www.cocoanetics.com/2012/05/resource-bundles/
* http://www.galloway.me.uk/tutorials/ios-library-with-resources/

### Subspecs

* Several static library targets
