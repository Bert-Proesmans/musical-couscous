# Musical Couscous

> **What's in a name?** that which we call a rose by any other name would smell as sweet
> > William Shakespeare

Testing grounds for 21st century app development, deployment and runtime maintainability of c# applications.

## Details

## Code

* Use intuitive names for types and variables, or at least names providing clarity and insight into the value
* Limit use of different namespaces. Files can be virtually organized through the project/solution file. Less
namespaces means less clutter.
* Keep code as much as possible inside the same project/assembly. Split off into a seperate assembly when there
is need to keep code seperated and visible to form a non circular dependency chain.
* Do not form circular dependencies! Use project structure to make this obvious and visible.
* Extension methods are sometimes as good or better than class methods.
* Handle null-values with monads, attempt to approach problems in a functional and therefore more explicit way.
Explicitness might lead to less concise code, but experience will help building better abstractions.
* Attempt to create as much [Pure](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.contracts.pureattribute?redirectedfrom=MSDN&view=netstandard-2.0) functions as possible!
* Use TODO, NOTE, WARN and ERROR within comments to clarify information that the code isn't explicitly mentioning. Do not
confuse these words with their counterparts in logging! Code comments are about code structure and algorithmic invariants.
  * `TODO(${Author}): ${Description}` to indicate skipped or additional work
  * `NOTE(${Author}): ${Description}` to indicate implicit information
  * `WARN(${Author}): ${Description}` to indicate handled edge cases
  * `ERROR(${Author}): ${Description}` to indicate unhandled edge cases and other input not explicitly handled/not conforming
algorithmic invariants.
    -> WARN and ERROR are rarely used but are mentioned should a specific case arise that tends to call for them.
* Make proper use of logging levels. TRACE, INFO, WARNING, ERROR and FATAL. Do not confuse these words with their
counterparts in code comments! Log levels are about tracing program flow and data validation. Make note that everything 
below FATAL is expected, FATAL is for unforeseen exceptions that must trigger some alert in principle.

* Entities and DTO's are different things! Your code exists to glue these things together so don't attempt to use
dynamic solutions to map between them.
* Entities should always exist in a valid state, use fallible operations and return some result structure!
* Approach objects as immutable unless the domain requires mutability of (certain) properties.
* Use domain specific error types, or at least more detailed exceptions than the default.
* Otherwise, wrap primitive data into structures that add semantics to the actual values!

* Reference equality (object.Equals) != logical equality (IEquatable.Equals) != structural equality (IStructuralEquatable.Equals).
**Note that logical equality is either value-based or identifier based.** We're stuck with object.Equals, so override object.Equals 
with an implementation that holds the most sense. Structural equality is NOT recursive, so delegate to the inner values when in doubt.

> C# v9 will introduce a lot more interesting stuff regarding record classes (=pure value objects) and immutability!

## Project setup

* Enable Nullable typechecks `<Nullable>enable</Nullable>`, note that using this feature requires some experience
because this is a compiler feature. There has never been a hard commit on banishing null values!
* Enable C#8 `<LangVersion>8.0</LangVersion>`. This automatically happens when targetting the `netcoreapp3.0`
framework, but not when targetting `NetStandardX.X`
* Code analyzers are a big help! catch whatever you can at compile time to have a hassle free runtime (as much as possible)
* Use a formatted logger framework
* Do not add different stuff to your project; alerting should be done in response to logged data, automatic updates should
be handled by a companion piece of software etc.
* Use test projects to verify the coded logic. There is a handy trick to generically expose private types to your test libraries
* For projects following SEMVER, use minimum and maximum version bounds. This makes life downstream easier!

### Build

* Make use of the repository properties when building projects
  * -p:VersionPrefix=1.0.3 
  * -p:VersionSuffix=beta (suffix present means nuget package is pre-release!)
  * -p:SourceRevisionId=(hash)
  * -p:RepositoryUrl=https://github.com/Bert-Proesmans/Musical-Couscous 
  * -p:RepositoryType=git 
  * -p:RepositoryBranch=develop
* -p:AssemblyVersion does not carry SEMVER semantics! Best to keep this value limited to "${major}.${minor}" format!
There is uncertainty about the way CLR should load dependencies (exact match or being lenient), so this advice will
probably ping-pong around a few times.
* -p:AssemblyFileVersion is the actual version property you care about! Follow SEMVER semantics and format 
in "${major}.${minor}.${patch}.${build}". Most people use ${build} to encode language codes or specific build system.
In global::System.Version terms these are named ${major}, ${minor}, 
${build}, ${revision} respectively.
* -p:InformationalVersion is NOT useful for automatic processing, only use it for DISPLAY purposes! It can
contain a random informational string.
