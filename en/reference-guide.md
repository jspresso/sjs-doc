# Preface

SJS (Sugar for Jspresso) is a Groovy DSL (Domain Specific Language) for the [Jspresso framework](http://ww.jspresso.org). SJS frees Jspresso power without having to deal with Spring XML to assemble your application. The source code is more compact yet more humanly readable, since it hides most of the Jspresso internals. To achieve this goal, SJS heavily relies on conventions to seamlessly implement Jspresso standards simplifying the whole authring process. However, SJS does not weaken Jspresso power nor its expressivity since you can always fall back to Spring XML authoring whenever required.

SJS has been written in [Groovy](http://groovy.codehaus.org). Groovy is a Java based dynamic language that is particularly well suited to [DSL](http://en.wikipedia.org/wiki/Domain-specific_programming_language) authoring. DSLs are domain oriented languages that focus on simplifying the expression of a design. In our case, SJS substitutes itself to Spring XML for Jspresso applications authoring. SJS is a simple declarative language that does not require any knowledge about Groovy. SJS based source files are natively managed by the Jspresso build, making SJS a first-class citizen of the Jspresso ecosystem and development lifecycle. SJS being Groovy based will bring another benefit : Jspresso will provide an SJS Eclipse pluging with syntax coloring and code completion that will be based on the excelent [Groovy-Eclipse](http://groovy.codehaus.org/Eclipse+Plugin) plugin.

SJS is actually made of 2 different DSLs :

-   SJS Domain deals with domain model authoring. This includes entities and their relationships, business rules and constraints. Jspresso-based domain models are rich, meaning that you can implement most of your business logic into the application model itself independently of the UI.

-   SJS Front deals with view, application structure, navigation and actions authoring.

During the build, SJS will generate Spring XML context files that describe the Jspresso application the "classic" way. During this generation phase, SJS controls the syntax, the cross references, the value types, so that errors get detected very early and the generated Spring XML unlikely wrong.

