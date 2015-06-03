# Using SJS

An SJS application description is a set of statements that are used to describe and assemble Jspresso components the same way you would do it using Spring XML. The big difference is the expression language itself.

# Authoring SJS source files

## General syntax

An SJS statement is made of 4 parts :

1. The name of the statement, e.g. :
2. The identifier of the underlying component, e.g. :
```groovy
Entity('Employee')
```
or even simpler without parenthesis :
```groovy
Entity 'Employee'
```
3. Arguments that configure the underlying component properties, e.g. :
```groovy
Entity('Employee', icon:'employee-48x48.png')
```
or even simpler without parenthesis :
```groovy
Entity 'Employee', icon:'employee-48x48.png'
```
4. Nested SJS statements between curly brackets (a "closure"). In that case, parenthesis cannot be omitted, e.g.
```groovy
Entity('Employee', icon:'employee-48x48.png') 
```
Or even with deeper nesting :
```groovy
split_vertical('Departments.and.teams.view',
                  top:'Company-departments.table',
                  cascadingModels:true) {
      bottom {
        split_horizontal (left:'Department-teams.table',
                          right:'Team-teamMembers.table',
                          cascadingModels:true)
      }
}
```
Statement nesting is a key feature for SJS flexibility.

Only statement name is always mandatory. Other parts may be required or not and SJS will enforce these rules.Some statements dont have any parameter. To summarize, every SJS statement conforms to the following scheme :

> name('identifier', parameters) {closure}

where closure is a list of SJS statements. This means that there can be an arbitrary nesting of SJS statements.

## Arguments types

The following types are used for SJS argument values :

- String : `'value'`
- Boolean : `[true|false]`
- Integer : `12`
- Decimal : `12.34`
- List : `['value1','value2']`
- Map: `[key1:'value1', key2:'value2']`

SJS will enforce argument types. Under certain circumstances, expected argument types are references themselves, in which case, SJS will check their validity. Used reference types can be :

- Domain elements, i.e. entities, components or interfaces.
- Domain element properties, i.e. fields.
- Views (form, table, split, ...).
- Actions and Action maps.
- Generic arbitrary beans for advanced usage.

## Arguments syntax

Arguments are defined using the following scheme :

> argumentName:argumentValue

For instance :
```groovy
icon:'traceable-48x48.png'
```

Whenever multiple arguments need to be declared, they are coma separated, e.g. :
```groovy
icon:'traceable-48x48.png', preferredWidth:45
```

An argument value can be a complex type, e.g. a list :
```groovy
uncloned:['createTimstamp','lasUpdateTimestamp']
```
or a map (associative array), e.g. :
```groovy
ordering:['name':'ASCENDING','firstName':'DESCENDING']
```

## Simplified forms

Language artifacts are sometimes optional. Omitting them can improve the reading.

### Parenthesis in statements

Whenever a statement does not contain a closure, parenthesis can be omitted, e.g. :
```groovy
Entity 'Employee', icon:'employee-48x48.png'
```

Whenever the closure is present, the identifier and arguments section must be surrounded by parentheses, e.g. :

```groovy
Entity('Employee', icon:'employee-48x48.png') {
    string_32 'name'
    string_32 'firstName'
}
```

### Square backets in lists

Whenever a list contains more than 1 element, it has to be surrounded by square backets, e.g. :

```groovy
Entity 'Employee', uncloned:['name','firstName']
```

Whenever a list contains only 1 element, square backets can be omitted, e.g. :
```groovy
Entity 'Employee', uncloned:'name'
```
### Properties composed in statement names

Some SJS statements allow for a property to be composed in their names in order to improve code readability :

- `string_n` and `text_n` where `n` is the maximum length of the string or text property, e.g. `string_32` ot `text_1024`.
- `split_vertical` and `split_horizontal` where vertical and horizontal are the allowed values for the split container orientation.

### View model inference

In order to simplify the code to write, a view model can be inferred from the view name. By convention, a view identifier should begin with the model identifier, followed by a dot, followed by an arbitrary complement, e.g. :

```groovy
form 'Employee.mainView'
```

If you follow this convention and if the model property is not set, SJS will look for a model with the identifier being the view id prefix. If it finds one, it will use it exactly as if the model property had been set. This means that if the `Employee` model exists, the following statements are equivalent :

```groovy
form 'Employee.mainView'
```
```groovy
form 'Employee.mainView', model:'Employee'
```
## Focus on the special *"custom"* argument

The argument named `custom` is a special argument that opens SJS built-in statements to further arbitrary customization. This argument is especially useful to customize generic action instances that are assigned to a view. Another important usage of the `custom` argument concerns the `bean` statement that allows to create instances of arbitrary classes.

The custom argument value is a map (associative array).

Supported map value types are :

- String : `'value'`
- Boolean : `[true|false]`
- Integer : `12`
- Decimal : `12.34`
- List : `['value1','value2']`
- Map: `[key1:'value1', key2:'value2']`

Regarding List and Map value types, the supported keys and values are :

- String : `'value'`
- Boolean : `[true|false]`
- Integer : `12`
- Decimal : `12.34`

As a consequence, it is impossible to declare to nest maps into a `custom` argument value. The following example shows a simple usage of the `custom` argument on an action definition :

```groovy
action 'chooseEmployeeAction',
    parent: 'lovAction',
    custom: [autoquery:false,
             entityDescriptor_ref:'Employee',
             initializationMapping:['company': 'company'],
             okAction_ref:'addFromList']
```

There are certain situations where you need to assign a reference to a custom property. On the previous example, the `okAction_ref` is such a property as it references another action whose identifier is `addFromList` (same applies for the `entityDescriptor_ref` property). In that case, SJS must be aware of the value being a reference to :

- generate correct Spring XML
- control the validity of the reference

Since the custom argument contains arbitrary properties, there is no way for SJS to know for the expected type of the value. That is why a reference property must be sufixed by **`_ref`** so that the value is interpreted as a reference to another component.

Whenever a list valued property is `_ref` suffixed, every of its values is interpreted as a reference.

A map valued property cannot be `_ref` suffixed itself. But each of its entry can be individually.

## Properties defined as arguments or closures

Some properties can be declared indifferently as an argument or as a closure. These properties are generally reference properties. Let's take an example.

A vertical split container references 2 views : 1 for the top part (the **"top"** property) and 1 for the bottom part (the **"bottom"** property).

You can simply reference existing views by using `top` and `bottom` arguments, e.g. :

```groovy
split_vertical 'Departments.and.teams.view',
    top:'Company-departments.table',
    bottom:'Department-teams.table'
```

Or you can use closures to define your top and bottom views inline, e.g. :

```groovy
split_vertical('Departments.and.teams.view') {
    top {
        table(parent:'Company-departments.table') {
            ...
        }
    }
    bottom {
        table(parent:'Department-teams.table') {
            ...
        }
    }
}
```

Or you can even mix both, e.g. :

```groovy
split_vertical('Departments.and.teams.view',
    top:'Company-departments.table') {
    bottom {
        table(...) {
            ...
        }
    }
}
```

Using a closure to define a property value allows :

- to inherit an existing component but override some of its configuration locally.
- to define brand new "anonymous" components inline.

Using a closure will often be referenced as an *inline* declaration. The main difference between an inline declaration and a top-level declaration is that the inline one is anonymous, i.e. it cannot be referenced from outside (using `_ref` suffix for instance). You will typically favorize inlined declarations for parts of the application that are hardly to be reused. On the other hand, top-level declarations are required to be named, thus can be later referenced.

## Arguments factorization

### paramSet

You will often face situations where you will want to name and reuse a set of arbitrary characteristics without introducing inheritance between your components. In order to achieve this goal, SJS provides the `paramSet` statement.

#### Declaration

As for every SJS statement, the `paramSet` statement can be named. However, unlike other statements its arguments are un-constrained and a paramSet statement does not allow for a closure.

Here is an example of a `paramSet` declaration :

```groovy
paramSet 'roMandatory', readOnly:true, mandatory:true
```

#### Usage

Every SJS statement allows for a `paramSets` argument that expects a list of paramSet identifiers. When performing generation, SJS will simply de-reference each paramSet and apply its arguments to the owning declaration.

Here is an example using the paramSet defined in the former section :

```groovy
string_32 'firstName', paramSets:['roMandatory']
```

which is equivalent to :

```groovy
string_32 'firstName',
    readOnly:true,
    mandatory:true
```

> **Note**
>
> As any SJS statement, `paramSet` allows for the `paramSets` argument ! This feature allows to nest paramSets between eachother.

Whenever the same argument is defined in different applied paramSets, the last one in the list wins, meaning that declared paramSets are applied in the order they are declared. This way, a paramSet value can always be overriden by the owning statement.

Sometimes, you will need to remove an argument from a paramSet when using it. Overriding a paramSet argument with a `null` value ill simply remove the argument from the paramSet scope.

`paramSet` and `paramSets` can be used in both SJS Domain and SJS Front DSLs.

### template

A template is a special type of paramSet that is implicitely applied to statements that have the same name as the template identifier.

#### Declaration

Here is an example of a template to be applied to the application forms :

```groovy
template 'form', parent:'decoratedView',
    labelsPosition:'ABOVE', columnCount:2
```

#### Usage

There is nothing explicit to code in order to use a template. Once the template of the former section has been declared, the following declarations are both equivalent :

```groovy
form 'Employee.pane'

form 'Employee.pane', parent:'decoratedView',
    labelsPosition:'ABOVE', columnCount:2
```

The *'form'* template as implicitely been applied to the `form` statement.

`template` is only supported in SJS Front DSL.

## Namespaces

Namespace usage allows to simplify SJS based source code by inferring package names of classes and resources that are produced. This package determination is based on conventions covering best practices about a Jspresso project layout.

A namespace is opened using the `namespace` statement, e.g. :

```groovy
namespace('org.jspresso.hrsample') {
    ...
}
```

The namespace will be applied to all statements that are placed into the `namespace` scope (between the curly braces).

Using the formerly defined namespace will make thefolowing declarations equivalent (note how the namespace has been applied to the image resource) :

```groovy
Entity 'City', icon:'city-48x48.png'

Entity 'org.jspresso.hrsample.model.City',
    icon:'classpath:org/jspresso/hrsample/images/city-48x48.png'
```

When a project is generated using the Jspresso archetype, a default namespace is created using the root package name. All declarations placed into standard SJS ource files are implicitely placed into this namespace.

Whenever SJS finds a "/" (slash) in a resource name or a "." (dot) in a class name, the enclosing namespace is ignored and the resource path or class name is considered absolute (i.e. not relative to the namespace).

Namespaces can be nested. In that case, nested namespace names are concatenated.

# Examples

This section gathers some high-level commented examples so that you can make your mind about SJS expressiveness for Jspresso applications authoring. To go further, you can refer to the SJS reference section of this manual as well as the Jspresso reference manual.

## SJS Domain

```groovy
Interface('Nameable') {
    string_64 'name', mandatory: true
}
```

Declares an interface Nameable to gather a common set of properties (in this case, the `name` property of type string, maximum length 64 characters and mandatory).

```groovy
Entity('Company', extend: 'Nameable',
       icon: 'company-48x48.png') {
    set 'employees', composition:true, ref:'Employee'
}
```

Declares a Company entity that implements the Nameable interface (thus inheriting the `name` property). Whenever an instance of Company needs to be graphically represented (in a tree view for instance), the `company-48x48.png` icon will be used.

The Company entity holds a collection of Employee entities (with a set semantics). It actually maps a 1-N relationship towards Employee that is further classified as being a composition (whenever a company is deleted from the system, all its employees are).

```groovy
Entity('Employee', extend: 'Nameable',
       processor:'EmployeePropertyProcessors',
       uncloned:['ssn'], icon:'male-48x48.png') {
    string_64 'firstName', mandatory:true, processors:'FirstNameProcessor'
    date 'birthDate'
    string_10 'ssn', regex:'[\\d]{10}', regexSample: '0123456789',
    reference 'company', ref: 'Company', mandatory: true, reverse:'Company-employees'
    set 'teams', ref: 'Team'
}
```

Declares an Employee entity that implements Nameable. Its `ssn` (social security number) is not cloned when the entity is. It's visually represented by the `male-48x48.png` icon.

A class EmployeePropertyProcessors is assigned to the Employee entity that will hold all property processors (see Jspresso reference documentation) that maintain the entity integrity and gets triggered . Each property processor will be declared on its owning property (`firstName` for instance).

Employee owns a `birthDate` date property and a `ssn` string property that is 10 characters max and validated by a regular expression. An employee can belong to multiple teams (Team entity) through the `teams` collection property.

The `company` reference property declares the reverse side of the bi-directional 1-N relationship between Company and Employee. This property is made mandatory so that an employee must belong to one and only one company. Note that the `Company-employees` identifier has implicitely been defined when you have declared the `employees` property on the Company entity. This is true for all collection properties (set or list) that are very often to be reused. This automatic identifier is generated using the pattern `[Entity]-[property name]`.

```groovy
Entity('Team', extend: 'Nameable',  
       icon: 'team-48x48.png') { 
    set 'teamMembers', ref:'Employee', reverse:'Employee-teams'
}
```

Declares a Team entity that implements Nameable. It's visually represented by the `team-48x48.png` icon.

Its `teamMembers` collection property declares the reverse side of the N-N bi-directional relationship between Team and Employee. Note how the implicit `Employee-teams` identifier is used to reference and connect the 2 sides of the relationship.

## SJS Front

```groovy
template 'form', parent:'decoratedView',
         labelsPosition:'ABOVE',
         columnCount:2
```

Declares a template for `form` statements. All new forms will have `decoratedView` as parent (a built-in abstract view with a titled border), labels above their matching fields and fields organized in 2 columns.

```groovy
form 'Employee.pane',
     fields: ['name', 'firstName', 'company.name'],
     widths: [name:2],
     description: 'employee.editing'
```

Declares a `form` on Employee. since it's a form, it inherits the form template characteristics. It will display 3 fields with the `name` field spanning the 2 columns (thus being alone on its row). Note the use of the dot nested notation (`company.name`) to reference compound properties. A tooltip will be installed on the form and Jspresso will use `employee.editing` as the internationalization key to look-up the translation of the tooltip based on the session user language.

```groovy
treeNode 'Company-departments.treeNode', render:'name'

treeNode 'Department-teams.treeNode', render:'name'

tree('Company.tree', render: 'name') {
    subTree('Company-departments.treeNode') {
        subTree('Department-teams.treeNode')
    }
}
```

Declares 2 tree nodes (tree levels), 1 for the company departments and 1 for the department teams. The relationships backing the tree nodes are inferred by convention from the identifiers. Each department and team nodes will use the underlying entity name as label (`render` argument). Each concrete tree node will leveraged the icon declaration of it model entity, i.e. Company, Department or Team.

```groovy
tabs 'Company.tab.pane',
    views: ['Company.pane', 'Company.tree']
```

Builds a tab pane with 2 tabs, the first tab displaying the `Company.pane` view (not described in this manual) and the second one displaying the `Company.tree` view.

```groovy
table 'Company-departments.table', actionMap:'masterDetail'
```

Declares a table displaying a company departments. The standard built-in `masterDetail` action map is assigned to the view.

```groovy
split_vertical 'Company.and.Departements.view',
    top:'Company.tab.pane',
    bottom:'Company-departments.table'
```

Declares a vertically oriented split pane. The top view of the split pane is the `Company.tab.pane` whereas the bottom view is the `Company-departments.table`.

# Key concepts

This section translates the key concepts that are detailed in the Jspresso reference manual into SJS terminology.

## Entity relationships

Entity relationships definition is only a matter of a few SJS statements. Please note that Jspresso leverages this declarations to seamlessly preserve your domain model integrity, especially when relationships are bi-dierectional (modifying 1 end of the relationship impacts the other end).

### Reference relationship (1 multiplicity)

A department belongs to a company.

```groovy
Entity('Company') {
  ...
}
```
```groovy
Entity('Department') {
  ...
  reference 'company' ref:'Company'
  ...
}
```

### Collection relationship (N multiplicity)

A company is made of an unordered collection of departments.

```groovy
Entity('Company') {
  ...
  set 'departments',
    ref:'Department'
  ...
}                     
```
```groovy
Entity('Department') {
  ...
}                     
```

> **Note**
>
> Whenever the `departments` collection should be an indexed collection (i.e. with element order persisted), you can use the `list` statement instead of the `set` statement.

### Bi-directional relationship

Both relationship ends defined above must be connected between each other.

```groovy
Entity('Company') {
  ...
  set 'departments',
    ref:'Department'
  ...
}
```
```groovy
Entity('Department') {
  ...
  reference 'company',
    ref:'Company',
    reverse:'Company-departments'
  ...  
}
```
As used above, SJS implicitely creates a top-level, identified and referenceable component for each relation end. The pattern used for auto generated identifiers is :

> **[OwningComponent]-[associationNameEnd]**

In the example above, the `Company-departments` relation end is referenced as the reverse end of the `company` one.

> **Note**
>
> The default generated relationship end identifier can be explicitely overriden by using the id argument.

Making a bi-directional association is allowed for any type of relationship end :

- a 1-1 association is defined by making 2 references reverse of each other.
- a 1-N association is defined by making a reference reverse of a collection. This is also the default type of association created when the association is not bi-directional.
- a N-N association is defined by making 2 collections reverse of each other.

For instance, if a department can be managed by an employee and an employee can manage at most a department, we can define the 1-1 association as follows :

```groovy
Entity('Employee') {
  ...
  reference 'managedDepartment',
    ref:'Department',
    reverse:'Department-manager'
  ...
}
```
```groovy
Entity('Department') {
  ...
  reference 'manager',
    ref:'Employee'
  ...
}
```

> **Note**
>
> You can connect 2 relationship ends by only setting the `reverse` argument on one of the ends. The other end is automatically updated.

## Business rules

There are several ways that are offered by Jspresso in order to inject business logic into the application domain model. All of them are described into the Jspresso reference manual. This section explains how you would translate these concepts using SJS.

### Lifecycle interceptors

Lifecycle interceptors allow to inject java code that will be executed on entity lifecycle events :

- `onCreate` (in-memory creation)
- `onPersist` (in perstsistent store creation)
- `onUpdate` (in persistent store update)
- `onDelete` (in persistent store deletion)
- `onLoad` (load from persistent strore)

This is how you can declare lifecycle interceptors using SJS statements :

```groovy
Interface('Traceable',
          interceptors:'TraceableLifecycleInterceptor') {
  date_time 'createTimestamp'
  date_time 'lastUpdateTimestamp'
}
```

in SJS, lifecycle interceptors packages are determined by convention using the following pattern :

> **[namespace].model.service**

Here is the code of the lifecycle interceptor (`TraceableLifecycleInterceptor.java`) , given that the application namespace is `org.jspresso.hrsample` :

```java
package org.jspresso.hrsample.model.service;

import ...

public class TraceableLifecycleInterceptor extends
    EmptyLifecycleInterceptor<Traceable> {

  @Override
  @SuppressWarnings("unused")
  public boolean onPersist(Traceable traceable, IEntityFactory entityFactory,
      UserPrincipal principal, IEntityLifecycleHandler entityLifecycleHandler) {
    traceable.setCreateTimestamp(new Date());
    return true;
  }

  @Override
  @SuppressWarnings("unused")
  public boolean onUpdate(Traceable traceable, IEntityFactory entityFactory,
      UserPrincipal principal, IEntityLifecycleHandler entityLifecycleHandler) {
    traceable.setLastUpdateTimestamp(new Date());
    return true;
  }
}
```

### Property processors

Property processors allow to inject java code that will be executed on property modification phases :

- `before` to check the incoming new property value and potentially reject it with a deailed explanation.
- `in-between` to perform some systematic transformation on the incoming value before actually passing it along.
- `after` to trigger some extra computation once the property has actually been modified.

As detailed in the Jspresso reference guide, SJS follows the convention of grouping all the property processors of an entity into an outer, englobing class. Each property processor (defined on the property level) is then an inner class of this containg class (defined on the entity level).

For instance, you can declare property processors like this :

```groovy
Entity('Employee',
        processor:'EmployeePropertyProcessors') {
    string_32 'firstName', processors:'FirstNameProcessor'
    date 'birthDate', processors:'BirthDateProcessor'
}
```

SJS determines property processors package by convention using the following pattern :

> **[namespace].model.processor**

Here is the code of the property processors containing class (`EmployeePropertyProcessors.java`) , given that the application namespace is `org.jspresso.hrsample` :

```java
package org.jspresso.hrsample.model.processor;

import ...

public class EmployeePropertyProcessors {

  /**
   * Birth date property processor.
   */
  public static class BirthDateProcessor extends
      EmptyPropertyProcessor<Employee, Date> {

    /**
     * Checks that the employee age is at least 18.
     * <p>
     * {@inheritDoc}
     */
    @Override
    public void preprocessSetter(Employee employee, Date newBirthDate) {
      if (newBirthDate == null
          || employee.computeAge(newBirthDate).intValue() < 18) {
        throw new IntegrityException("Age is below 18", "age.below.18");
      }
    }
  }

  /**
   * First name property processor.
   */
  public static class FirstNameProcessor extends
      EmptyPropertyProcessor<Employee, String> {

    /**
     * Formats the new first name. The formatting is :
     * <li>Capitalize the 1st letter
     * <li>Lower case all the other letters
     * <p>
     * {@inheritDoc}
     */
    @Override
    public String interceptSetter(Employee employee, String newFirstName) {
      if (newFirstName != null && newFirstName.length() > 0) {
        StringBuffer formattedName = new StringBuffer();
        formattedName.append(newFirstName.substring(0, 1).toUpperCase());
        formattedName.append(newFirstName.substring(1).toLowerCase());
        return formattedName.toString();
      }
      return super.interceptSetter(employee, newFirstName);
    }
  }
}
```

### Services

Arbitrary services can be declared and implemented on entities. A service defnition is made of :

- an interface that specify the service.
- an implementation class that implements the service code itself.

A service can implement several methods and an entity can implement several services. Services are declared on an entity using a map (associative array). The map keys are the services interfaces and the map values are the matching services implementation classes.

For instance, you can declare services like this :

```groovy
Entity ('Employee',
         services:[EmployeeService:'EmployeeServiceDelegate']) {
  ...
}
```

SJS determines services package by convention using the following pattern :

> **[namespace].model.service**

Here is the code of the service interface (`EmployeeService.java`) , given that the application namespace is `org.jspresso.hrsample` :

```java
package org.jspresso.hrsample.model.service;

import ...

public interface EmployeeService {

  /**
   * Computes the employee age.
   * 
   * @param birthDate
   *            the employee birth date.
   * @return the computed age based on the birth date or null if the birth date
   *         is not available.
   */
  Integer computeAge(Date birthDate);
}

and service implementation (`EmployeeServiceDelegate.java`) :

package org.jspresso.hrsample.model.service;

import ...

public class EmployeeServiceDelegate implements IComponentService {

  /**
   * Computes the employee age.
   * 
   * @param employee
   *          the employee this service execution has been triggered on.
   * @param birthDate
   *          a birth date (might be different than the actual employee birth
   *          date).
   * @return the age computed from the birth date passed as parameter.
   */
   public Integer computeAge(Employee employee, Date birthDate) {
    if (birthDate != null) {
      return new Integer(
          (int) ((new Date().getTime() - birthDate.getTime())
                 / (1000L * 60 * 60 * 24 * 365)));
    }
    return null;
  }
}
```

> **Note**
>
> The service implementation class does not itself implement the service interface. This is discussed in the Jspresso reference manual.

### Computed properties

Jspresso allows to define computed properties on entities. These computed properties are calculated using some java code that is injected in the entity definition. As described in the Jspresso reference manual, all computed properties should be grouped in a single class that is used as an "extension" of the core entity.

For instance, you can declare entity computed properties like this :

```groovy
Entity ('Employee',
         extension:'EmployeeExtension') {
  date 'birthDate'
  integer 'age', computed:true
}
```

SJS determines extension class package by convention using the following pattern :

> **[namespace].model.extension**

Here is the code of the extension class (`EmployeeExtension.java`) , given that the application namespace is `org.jspresso.hrsample` :

```java
package org.jspresso.hrsample.model.extension;

import ...

public class EmployeeExtension extends
    AbstractComponentExtension<Employee> {

  /**
   * Constructs a new <code>EmployeeExtension</code> instance.
   * 
   * @param extendedEmployee
   *            The extended Employee instance.
   */
  public EmployeeExtensionSimple(Employee extendedEmployee) {
    super(extendedEmployee);
  }

  /**
   * Computes the employee age.
   * 
   * @return The employee age.
   */
  public Integer getAge() {
    return getComponent().computeAge(getComponent().getBirthDate());
  }
}
```

Notice how the `computeAge` service defined in the former section is used to compute the `age` property.

## Actions

Actions are application components that allow the user to interact with the application. An action can trigger any type of behaviour like, e.g. :

- creating an entity and adding it to a collection
- generating a report and displaying it to the user
- opening a transaction to perform some complex operation on the domain model
- navigate programmatically between modules and workspaces
- *[place your own action here]*...

Jspresso offers tens of built-in standard actions that can be reused or even assembled to make more complex ones.

Most of the time, an action is materialized as a UI component (generally a button) using an icon and/or a label and/or a descriptive tooltip. However, actions can be faceless, e.g. module startup actions, list double-click actions, ...

### Action structure

An action is a piece of behavioural code by itself but it also provides a structure that makes it suitable for combining : actions can be chained together.

Chaining occurs when an action is registered on another one as :

- `next` action, i.e. the execution flow passes definitively to the next action.
- `wrapped` action, i.e. the execution flow comes back to the original action once the wrapped action has finished its execution.

Actions are classified into 2 main categories that influence the way they can be chained together :

- frontend actions act on the frontend layer, i.e. they can deal with the UI.
- backend actions are UI agnostic, i.e. they only deal with the domain or backend services.

All chaining combination rules are valid except that frontend actions cannot be chained (`next` or `wrapped`) behind backend actions. In general, backend actions are chained `next` another backend action or `wrapped` a frontend action.

### Action lists and action maps

Actions are grouped into action lists that in turn are grouped into action maps. Every view can be assigned an action map. Most of the time, an action map is visually represented as a toolbar. Jspresso comes with standard built-in action maps that can be used as-is on views. For insance the `masterDetail` built-in action map contains the actions to work on a master-detail UI.

Using SJS, action maps can be assigned to views using the `actionMap` argument, e.g. :

```groovy
table 'Company-departments.table',
       actionMap:'masterDetail'
```

Actions can be inherited and customized, e.g. :

```groovy
action 'addFromList',
       parent:'lovOkFrontAction',
       next:'addAnyToMasterFrontAction'
```

In the example above, the *addFromList* action inherits the built-in *front.lov.ok*action and chains the built-in *front.any.add* action as `next` action.

You can also define new action maps that use existing actions or even declares new ones inline, e.g. :

```groovy
actionMap {
    actionList('EDIT') {
        action(parent:'lovAction',
               custom:[
                   autoquery:false,
                   entityDescriptor_ref:'Employee',
                   initializationMapping:['company':'company'],
                   okAction_ref:'addFromList'
               ])
        action(ref:'removeAnyCollectionFromMasterFrontAction')
    }
}
```

Actions can be written in plain java or groovy.

## Arbitrary beans

### bean, set, list and map

SJS Front provides a general, loosely-typed syntax to instanciate arbitrary java beans that can be referenced afterwards in standard SJS components. This is particularly useful to perform arbitrary customizations that would not be otherwize possible due to the evident limitation of the strongly typed SJS syntax.

SJS statements that compose this feature are `bean`, `set`, `list` and `map`.Their meaning is very close to their Spring companions, i.e. `<bean>`, `<set>`, `<list>` and `<map>` tags.

The `bean` statement provides the following arguments :

- an identifier
- `parent` : the parent component identifier.
- `class` : the bean fully-qualified class name.
- `custom` : a map of custom properties (refer to the related documentation chapter).

`bean` statements can be nested; in that case, the nested bean definition is considered a property of the enclosing one. `list`, `set` and `map` statements are always declared into the bean statement closure. in other words, they cannot be made top-level components and thus individually referenced from outside.

Here is a quite complex bean definition :

```groovy
bean('siteArchiveConfigSet',
     class:'com.bluevox.cms.backend.action.AbstractResourceArchiveAction$ConfigSet',
     custom:[siteFileName:'pages.xml']) {
  bean('siteDumper', class:'com.bluevox.cms.backend.action.SiteDumper',
       custom:[
         templateResourcePath:'/com/bluevox/cms/backend/action',
         templateName:'XmlSiteDumper.ftl'
       ]
  )  
}
```

### The bean identifier

A bean identifier is interpreted differently depending on the position of the `bean` statement.

When the `bean` statement is a top-level statement, the identifier is used the classic way. It allows to reference the bean in other SJS components.

When the `bean` statement is inlined into another component closure, the identifier is leveraged as being the property name of the enclosing component.

When the `bean` statement is declared into the `map` statement closure, the identifier becomes the map key under which the bean is stored. This behaviour is also true for any SJS component defined inline into the `map` statement.

When the `bean` statement is declared into the `list` or `set` statement closure, no identifier is allowed.

> **Note**
>
> Although `bean`, `set`, `list` and `map` break the limits, they require a relatively good knowledge of the Spring DI container so that the generated code fulfills the requirements. SJS won't be of any help for controlling the actual runtime types of the beans.

# SJS source files organization

## Default organization

When a project is initialized using the Jspresso application archetype, 5 standard SJS source files are created :

- `application.groovy` is a wrapper for the other files. You will rarely modify it unless you want to change the default project setup (to split the SJS files in functional domains for instance).
- `model.groovy` contains the application domain model (compiled using SJS Domain).
- `view.groovy` contains the application views (compiled using SJS Front).
- `frontend.groovy` contains everything that is frontend related but distinct from the views, e.g. modules and workspaces for instance (compiled using SJS Front).
- `backend.groovy` contains everything that is backend related but distinct from the domain model, e.g. backend actions for instance (compiled using SJS Front).

All the boiler plate SJS code is already written in `application.groovy`. This means that you don't have to deal with the application namespace, error reporting, and so on. All you have to do is directly write you application SJS code into the pre-initialized SJS files. For instance, the following declaration in `model.groovy` is enough to generate an entity :

```groovy
Entity('Person') {
  string_32 'name'
  integer 'age'
}
```

## Further modularization

You can further modularize SJS applications using the `include` statement :

```groovy
include('filename.groovy')
```

included files are merged exactly as if all their statements had been written directly into the in the including SJS source file. This is actually the same mecanism that is used by the `application.groovy` wrapper to include the standard SJS source files.

## Identifiers unicity scope and "spec" section

As a general rule, component identifers must be unique across the appliction. If 2 components are declared using the same identifier, SJS will raise a compilation error duringthe build.

However, SJS allows an advanced usage in order to open specific scopes in which you can define components with the same identifier. A typical usage of this feature is to declare different implementation for the same identifier depending on the UI channel (e.g. Qooxdoo vs Flex vs Swing). This specific scopes feature is only available for SJS Front and not for SJS Domain.

Opening a specific scope is achieved using the `spec` statement, e.g. :

```groovy
spec('qooxdoo') {
    ...
}
spec('flex') {
    ...
}
spec('swing') {
    ...
}
```

All the declarations that are placed inside the spec closure belong to this scope.

Contrary to what you might believe, components that are declared in a specific scope are visible from their inner *AND* outer scopes. They are only isolated from their same level scopes.

Scopes meaning are not limited to UI based customization. You could define staging scopes like *development* *pre-production* and *production*.

Scopes are then used in `application.groovy` to write specific Spring XML context files that are afterwards used to compose specific Spring contexts to declare in `beanRefFactory.xml`.

In `application.groovy` :

```groovy
frontendBuilder.writeOutputFile('swing',
     project.properties['outputDir'],
     'swing-'+project.properties['viewOutputFileName'])
```

This will produce a custom `swing-dsl-view.xml` Spring context file in the build output directory. This custom Spring context file is then available for inclusion in a custom Spring context.

In `beanRefFactory.xml` :

```xml
  <bean
    id="hrsample-swing-context"
    class="org.springframework.context.support.ClassPathXmlApplicationContext"
    lazy-init="true">
    <constructor-arg>
      <list>
        ...
        <value>org/jspresso/hrsample/spec/swing-dsl-view.xml</value>
        ...
      </list>
    </constructor-arg>
  </bean>
```