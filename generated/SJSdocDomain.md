#### <a name=""></a>Reference for SJS Domain
#### <a name=""></a>Root
#### <a name=""></a>Domain


+ **mandatory** : `projectName`
+ **allowed next element** : `Entity, Interface, Component, external, paramSet`

This root descriptor is used in the file application.groovy generated during the initialization of the project by Jspresso. There is generally no need to change it.


<table>
<caption>Domain properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>projectName</strong></p><p><code>String</code></p>
</td>
<td><p>Project name</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>mute</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Lets say if SJS is verbose or not during the generation process</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>includeDirectory</strong></p><p><code>String</code></p>
</td>
<td><p>indicates in which directory are the include files</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>Component
#### <a name=""></a>Entity


+ **allowed previous element** : `Domain`
+ **allowed next element** : `string, text, imageUrl, password, integer, date, bool, decimal, time, duration, percent, enumeration, typeEnumeration, range, refId, color, binary, image, java, any, sourcecode, html, reference, list, set, paramSet`
+ **Jspresso** : `BasicEntityDescriptor`

This descriptor key to the description of the application model. It is used to describe a model entity.

The description of an entity can be very simple :
<pre>Entite('person'){

  string_32 'name'

  string_32 'firstName'

  integer age
}</pre>
or be richer to meet more complex needs :
<pre>Entity('Department', extend: 'OrganizationalUnit',

        icon: 'department-48x48.png',

        rendered: ['ouId', 'name', 'manager', 'contact',

        'createTimestamp', 'lastUpdateTimestamp']) {

  reference 'company', ref: 'Company', reverse: 'Company-departments', 
          mandatory: true, composition: true

  set 'teams', ref: 'Team', composition: true

}</pre>
<br>Note : Concurrent access conflicts are automatically manage by Jspresso through optimistic locking
</br>


<table>
<caption>Entity properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>extend</strong></p><p><code>ListOfString</code></p><p><code>ancestorDescriptors</code></p>
</td>
<td><p>Registers this Entity with a collection of ancestors. It directly translates the components inheritance hierarchy since the component
property descriptors are the union of the declared property descriptors of the component and of its ancestors one.
A component may have multiple ancestors which means that complex multiple-inheritance hierarchy can be mapped
<pre>Interface('Nameable') { string_64 'name', mandatory: true }

Entity('City', <b>extend</b>: 'Nameable'){...}</pre></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>description</strong></p><p><code>String</code></p>
</td>
<td><p>Sets the description of this descriptor. Most of the descriptor descriptions
are used in conjunction with the Jspresso i18n layer so that the
description property set here is actually an i18n key used for translation.
Description is mainly used for UI (in tooltips for instance) but
may also be used for project technical documentation, contextual
help, ...</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>icon</strong></p><p><code>String</code></p><p><code>iconImageURL</code></p>
</td>
<td><p>Sets the icon image URL of this descriptor. Supported URL protocols include :
<ul>
<li>all JVM supported protocols
</li>
<li>the jar:/ pseudo URL protocol
</li>
<li>the classpath:/ pseudo URL protocol
</li>
</ul>
<pre>Interface('Traceable', <b>icon</b>: 'traceable-48x48.png'){...}</pre></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>iconWidth</strong></p><p><code>Integer</code></p><p><code>iconPreferredWidth</code></p>
</td>
<td><p>Allows to define a custom width for the icon. In that case, the default dimension computed by the view
factory is ignored.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>iconHeight</strong></p><p><code>Integer</code></p><p><code>iconPreferredHeight</code></p>
</td>
<td><p>Allows to define a custom height for the icon. In that case, the default dimension computed by the view
factory is ignored.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>rendered</strong></p><p><code>ListOfRefField</code></p><p><code>renderedProperties</code></p>
</td>
<td><p>This property allows to define which of the component properties are
to be rendered by default when displaying a UI based on this component
family. For instance, a table will render 1 column per rendered
property of the component. Any type of property can be used except
collection properties. Since this is a List queriable properties are
rendered in the same order.
Whenever this property is null (default value) Jspresso determines
the default set of properties to render based on their types, e.g. ignores
collection properties.
Note that this property is not inherited by children descriptors, i.e.
even if an ancestor defines an explicit set of rendered properties, its
children ignore this setting.
<pre>Entity('Employee',
 
<b>rendered</b>: ['name', 'firstName', 'ssn', 'age']){...}</pre></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>queryable</strong></p><p><code>ListOfRefField</code></p><p><code>queryableProperties</code></p>
</td>
<td><p>This property allows to define which of the component properties are
to be used in the filter UIs that are based on this component family (a
QBE screen for instance). Since this is a List queriable properties
are rendered in the same order.
Whenever this this property is null (default value), Jspresso chooses
the default set of queryable properties based on their type. For instance,
collection properties and binary properties are not used but
string, numeric, reference, ... properties are. A computed property
cannot be used since it has no data store existance and thus cannot
be queried upon.
Note that this property is not inherited by children descriptors, i.e.
even if an ancestor defines an explicit set of queryable properties, its
children ignore this setting
<pre>Entity('Employee',
 
<b>queryable</b>: ['name', 'firstName', 'ssn', 'age']){...}</pre></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>uncloned</strong></p><p><code>ListOfRefField</code></p><p><code>unclonedProperties</code></p>
</td>
<td><p>Configures the properties that must not be cloned when this component
is duplicated. For instance, tracing informations like a created
timestamp should not be cloned; a SSN neither. For a given component,
the uncloned properties are the ones it defines augmented by
the ones its ancestors define. There is no mean to make a component
property clonable if one of the ancestor declares it un-clonable.
<pre>Entity('Employee', <b>uncloned</b>: ['managedOu', 'ssn']){...}</pre></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>ordering</strong></p><p><code>Map</code></p><p><code>orderingProperties</code></p>
</td>
<td><p>Ordering properties are used to sort un-indexed collections of instances
of components backed by this descriptor. This sort order can
be overridden on the finer collection property level to change the way
a specific collection is sorted. This property consist of a Map whose
entries are composed with the property name as key and the sort order for this property as String "ASCENDING" or "DESCENDING"
<pre>Entity('Employee', <b>ordering</b>: [name: "ASCENDING"]){...}</pre></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>pageSize</strong></p><p><code>Integer</code></p>
</td>
<td><p>Whenever a collection of this component type is presented in a pageable
UI, this property gives the size (number of component instances)
of one page. This size can usually be refined at a lower level (e.g. at
reference property descriptor for "lists of values"). A null value (default)
disables paging for this component.
<pre>Entity('Employee', <b>pageSize</b>: 10){...}</pre></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>sqlName</strong></p><p><code>String</code></p>
</td>
<td><p>Instructs Jspresso to use this name when translating this component
type name to the data store namespace. This includes , but is not
limited to, database table names.
By default Jspresso uses its default naming policy
<pre>Entity('Employee', <b>sqlName</b>: "T_EMPLOYEE"){...}</pre></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>grantedRoles</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>Assigns the roles that are authorized to manipulate components
backed by this descriptor. This will directly influence the UI behaviour
and even composition. Note that this authorization enforcement does not prevent programatic
access that is of the developer responsbility.
<pre>Entity('Employee',<b>grantedRoles</b>: ['administrator', 'manager']){...}</pre></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>booleanWritabilityGates</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>Assigns a collection of gates to determine component writability. A component will
be considered writable (updatable) if and only if all booleanGates gates are open.
<pre>
entity 'invoice', booleanWritabilityGates: ['val1', '!val2']
 
</pre>
<ul>
<li>The first 'val1' gate is open if the val1 property is true on the underlying model
</li>
<li>The second '!val2' gate is open if val2 is false on the underlying model
</li>
</ul>


This mecanism is mainly used for dynamic UI authorization based on
model state, e.g. a validated invoice should not be editable anymore.
component assigned gates will be cloned for each component instance created
and backed by this descriptor. So basically, each component instance will
have its own, unshared collection of writability gates.


By default, component descriptors are not assigned any gates collection,
i.e. there is no writability restriction. Note that gates do not enforce
programatic writability of a component; only UI is impacted.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>rolesWritabilityGates</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>Assigns a collection of gates to determine component writability. A component will
be considered writable (updatable) if and only if all RolesGates gates are open.
<pre>
entity 'invoice', rolesWritabilityGates: ['role1', '!role2']
</pre>
<ul>
<li>The first gate 'role1' is open if the connected user has the role1
</li>
<li>The second gate '!role2' is open if the connected user does not have the role2
</li>
</ul>
Same mecanism has booleanWritabilityGates</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>extension</strong></p><p><code>String</code></p>
</td>
<td><p>Instructs the framework that a delegate class is attached to this component to implements
getters for his computes properties.
Properties defined with a computed = true are computed whith a setter defined in that delegate class.


Delegate instances are stateful. This allows for some caching of computing
intensive properties. Delegate instances are lazily created when needed, 
i.e. when the computed property is accessed either programmatically or by the binding layer.
The delegate class must implement the IComponentExtension
&lt;T&gt; interface (where &lt;T&gt; is assignable from the owning component
class) and provide a public constructor taking exactly 1 parameter :
the component instance. Jspresso provides an adapter class to inherit
from : AbstractComponentExtension&lt;T&gt; . This helper class
provides the methods to access the enclosing component from the
delegate implementation as well as the Spring context it comes from,
when needed.

<pre>Entity('Employee', <b>extension</b>: 'EmployeeExtension'){

 integer 'age', <b>useExtension</b>: true
}</pre></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>processor</strong></p><p><code>String</code></p>
</td>
<td><p>Class name in which all class processors associated with the properties of this component are grouped
<pre>Entity('Employee', 
<b>processor:</b>'EmployeePropertyProcessors'){...}</pre></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>interceptors</strong></p><p><code>ListOfString</code></p><p><code>lifecycleInterceptorClassNames</code></p>
</td>
<td><p>List of lifecycle interceptor instances that will be triggered on the different phases of the
component lifecycle : when the component is instanciated in memory or when the component is created,
updated, loaded or deleted in the data store.
<pre>Interface('Traceable',
<b>interceptors</b>: 'TraceableLifecycleInterceptor'){...}</pre></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>interceptorBeans</strong></p><p><code>ListOfString</code></p><p><code>lifecycleInterceptorBeanNames</code></p>
</td>
<td><p>This is the same as the interceptors list except that interceptor instances are declared using their identfiers.
This allows to use externally configured instances instead of instances that are created at runtime using their
default constructor.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>services</strong></p><p><code>Map</code></p><p><code>serviceDelegateClassNames</code></p>
</td>
<td><p>Much the same as serviceDelegateBeanNames except that instead
of providing a map valued with Spring bean names, you provide
a map valued with fully qualified class names. These class
must :
<ul>
<li>provide a default constructor
</li>
<li>implement the IComponentService marker interface.
</li>
</ul>
When needed, Jspresso will create service delegate instances.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>serviceBeans</strong></p><p><code>Map</code></p><p><code>serviceDelegateBeanNames</code></p>
</td>
<td><p>Registers the collection of service delegate instances attached to this
component. These delegate instances will automatically be triggered
whenever a method of the service interface it implements get executed.For instance :
<ul>
<li>the component interface is MyBeanClass. It implements the service
interface MyService.
</li>
<li>the service interface MyService contains method int
foo(String).
</li>
<li>the service delegate class, e.g. MyServiceImpl must implement
the method int foo(MyBeanClass,String). Note that the parameter
list is augmented with the owing component type as 1st
parameter. This allows to have stateless implementation for delegates,
thus sharing instances of delegates among instances of
components.
</li>
<li>when foo(String) is executed on an instance of MyBeanClass,
the framework will trigger the delegate implementation, passing the
instance of the component itself as parameter.
</li>
</ul>
This property must be set with a map keyed by service interfaces
and valued by Spring bean names (i.e. Spring ids). Each bean name
corresponds to an instance of service delegate. When needed, Jspresso
will query the Spring application context to retrieve the delegate
instances. This property is equivalent to setting serviceDelegateClassNames
except that it allows to register delegate instances
that are configured externally in the Spring context. lifecycle interceptor
instances must implement the IComponentService marker interface.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>parent</strong></p><p><code>Ref</code></p>
</td>
<td><p>Parent property allows to used an other descriptor as a model to override certain properties. This ability directly
resulting from Spring configuration is generally not used in the model</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>toString</strong></p><p><code>RefField</code></p><p><code>toStringProperty</code></p>
</td>
<td><p>Allows to customize the string representation of a component instance.
The property name assigned will be used when displaying the component instance as a string.
It may be a computed property that composes several other properties in a human friendly format. 
Whenever this property is null, the following rule apply to determine the toString property : 
<ul>
<li>the first string property from the rendered property
</li>
<li>the first rendered property if no string property is found among them
</li>
</ul>
Note that this property is not inherited by children descriptors, i.e.
even if an ancestor defines an explicit toString property, its children
ignore this setting.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>toHtml</strong></p><p><code>RefField</code></p><p><code>toHtmlProperty</code></p>
</td>
<td><p>Allows to customize the HTML representation of a component instance. The property name assigned will be used when displaying
the component instance as HTML. It may be a computed property that composes several other properties in a human friendly format.

Whenever this property is null, the toStringProperty is used. Note that this property is not inherited by children descriptors,
i.e. even if an ancestor defines an explicit toHtmlProperty property, its children ignore this setting.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>autoComplete</strong></p><p><code>RefField</code></p><p><code>autoCompleteProperty</code></p>
</td>
<td><p>Allows to customize the property used to autocomplete reference fields on
this component.
Whenever this property is null, the following rule apply to
determine the lovProperty :
<ul>
<li>the toString property if not a computed one
</li>
<li>the first string property from the rendered property
</li>
<li>the first rendered property if no string property is found among them
</li>
</ul>
Note that this property is not inherited by children descriptors, i.e. even
if an ancestor defines an explicit lovProperty, its children ignore
this setting.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>purelyAbstract</strong></p><p><code>Boolean</code></p>
</td>
<td><p>This property is used to indicate that the entity type described is to be
considered abstract. Jspresso will prevent any instanciation through
its generic actions or internal mecanisms. Trying to do so will result in
a low level exception and reveals a coding (assembling) error.

However, an abstract entity will have a concrete representation in the
data store that depends on the inheritance mapping strategy used
. As of now, Jspresso uses the join-subclass inheritance mapping
strategy when generating the Hibernate mapping so an abstract entity
will end up as a table in the data store.
<pre>Entity('OrganizationalUnit', <b>purelyAbstract</b>: true){...}

Entity('Department', extend: 'OrganizationalUnit'){...} </pre></p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>Interface


+ **extend** : `Entity`
+ **Inherited properties ** : `extend, description, icon, iconWidth, iconHeight, rendered, queryable, uncloned, ordering, pageSize, sqlName, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, extension, processor, interceptors, interceptorBeans, services, serviceBeans, parent, toString, toHtml, autoComplete, purelyAbstract`
+ **allowed previous element** : `Domain`
+ **allowed next element** : `string, text, imageUrl, password, integer, date, bool, decimal, time, duration, percent, enumeration, typeEnumeration, range, refId, color, binary, image, java, any, sourcecode, html, reference, list, set, paramSet`
+ **Jspresso** : `BasicInterfaceDescriptor`

This descriptor is a mean of factorizing state/behaviour among components, entities or even sub-interfaces.
This is a much less coupling mecanism than actual entity inheritance and can be used across entities
that don't belong the the same inheritance hierarchy, or even accross types (entities, components, interfaces).
<pre><b>Interface</b>('Nameable') { string_64 'name', mandatory: true }


<b>Interface</b>('Traceable',

        interceptors: 'TraceableLifecycleInterceptor',

        uncloned: ['createTimestamp', 'lastUpdateTimestamp'],

        icon: 'traceable-48x48.png') {

  date_time 'createTimestamp', paramSets: 'readOnly', parent: 'City'

  date_time 'lastUpdateTimestamp', readOnly: true

}


Entity('Employee', <b>extend</b>: ['Nameable', 'Traceable']){

  ...

}</pre>


<table>
<caption>Interface properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>Component


+ **extend** : `Entity`
+ **Inherited properties ** : `extend, description, icon, iconWidth, iconHeight, rendered, queryable, uncloned, ordering, pageSize, sqlName, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, extension, processor, interceptors, interceptorBeans, services, serviceBeans, parent, toString, toHtml, autoComplete, purelyAbstract`
+ **allowed previous element** : `Domain`
+ **allowed next element** : `string, text, imageUrl, password, integer, date, bool, decimal, time, duration, percent, enumeration, typeEnumeration, range, refId, color, binary, image, java, any, sourcecode, html, reference, list, set, paramSet`
+ **Jspresso** : `BasicComponentDescriptor`

structures that are to be reused but don't have enough focus for being considered as entities. For instance
MoneyAmount component could be composed of a decimal and a reference to a Money entity.
This structure could then be reused in other elements of the domain like an Invoice or an Article.
Jspresso terminology for these type of structures is "Inlined Component".
Another example could be reused of contact informations
<pre>Component('ContactInfo') {

  string_256 'address'

  reference 'city', ref: 'City'

  string_32 'phone'

  string_128 'email', regex: "*", regexSample: 'contact@acme.com'

}



Entity('Employee') {

  ...

  reference 'contact', ref: 'ContactInfo'

  ...

}</pre>


<table>
<caption>Component properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>Common
#### <a name=""></a>common

This descriptor is an internal s SJS descriptor which is never used by the application.It s used by SJS to factorize
commons properties.


<table>
<caption>common properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>description</strong></p><p><code>String</code></p>
</td>
<td><p>Sets the description of this descriptor. Most of the descriptor descriptions
are used in conjunction with the Jspresso i18n layer so that the
description property set here is actually an i18n key used for translation.
Description is mainly used for UI (in tooltips for instance) but
may also be used for project technical documentation, contextual
help, ...</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>icon</strong></p><p><code>String</code></p><p><code>iconImageURL</code></p>
</td>
<td><p>Allows to define an icon to represent this property.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>iconWidth</strong></p><p><code>Integer</code></p><p><code>iconPreferredWidth</code></p>
</td>
<td><p>Allows to define a custom width for the icon. In that case, the default dimension computed by the view
factory is ignored.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>iconHeight</strong></p><p><code>Integer</code></p><p><code>iconPreferredHeight</code></p>
</td>
<td><p>Allows to define a custom height for the icon. In that case, the default dimension computed by the view
factory is ignored.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>i18nNameKey</strong></p><p><code>String</code></p>
</td>
<td><p>Sets i18n key used for translation if it is different from the description</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>mandatory</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Declare a property as mandatory. This will enforce mandatory checks when the owning component is persisted as well
as when the properties updated individually. Moreover, this information allows the views bound to the property to be
configured accordingly, e.g. display the property with a slightly modified label indicating it is mandatory.
This constraint is also enforced programmatically.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>readOnly</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Enforces a property to be read-only. This is only enforced at the UI level, i.e. the property can still be updated
programmatically. The UI may take decisions like changing textfields into labels if it knows the underlying
property is read-only</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>preferredWidth</strong></p><p><code>Integer</code></p>
</td>
<td><p>This property allows for setting an indication of width for representing
this property in a view.


Default value is null, so that the view factory will make its decision
based on the type and/or other characteristics of the property (e.g.
max length).</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>computed</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Properties defined with a useExtension = true are computed with a getter. This getter is defined in the delegate class attached 
by the <b>extension</b> property to the Entity, Component or Interface.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>sqlName</strong></p><p><code>String</code></p>
</td>
<td><p>Instructs Jspresso to use this name when translating this component
type name to the data store namespace. This includes , but is not
limited to, database table names.
By default Jspresso uses its default naming policy</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>parent</strong></p><p><code>Ref</code></p>
</td>
<td><p>parent property allows to used an other descriptor as a model and to override certain properties.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>id</strong></p><p><code>String</code></p>
</td>
<td><p>id created an identifier for the property. This identifier can then be referenced by other descriptors using refId</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>grantedRoles</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>Assigns the roles that are authorized to manipulate the property
backed by this descriptor. This will directly influence the UI behaviour
and even composition (e.g. show/hide columns or fields). Setting the
collection of granted roles to null (default value) disables role based
authorization on this property level. Note that this authorization enforcement
does not prevent programmatic access that is of the developer
responsbility.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>booleanWritabilityGates</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>Assigns a collection of gates to determine component writability. A component will
be considered writable (updatable) if and only if all booleanGates gates are open.
<pre>
entity 'invoice', booleanWritabilityGates: ['val1', '!val2']
 
</pre>
<ul>
<li>The first 'val1' gate is open if the val1 property is true on the underlying model
</li>
<li>The second '!val2' gate is open if val2 is false on the underlying model
</li>
</ul>
This mecanism is mainly used for dynamic UI authorization based on
model state, e.g. a validated invoice should not be editable anymore.
component assigned gates will be cloned for each component instance created
and backed by this descriptor. So basically, each component instance will
have its own, unshared collection of writability gates.


By default, component descriptors are not assigned any gates collection,
i.e. there is no writability restriction. Note that gates do not enforce
programatic writability of a component; only UI is impacted.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>rolesWritabilityGates</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>Assigns a collection of gates to determine component writability. A component will
be considered writable (updatable) if and only if all RolesGates gates are open.
<pre>
entity 'invoice', rolesWritabilityGates: ['role1', '!role2']
</pre>
<ul>
<li>The first gate 'role1' is open if the connected user has the role1
</li>
<li>The second gate '!role2' is open if the connected user does not have the role2
</li>
</ul>
Same mecanism has booleanWritabilityGates</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>unicityScope</strong></p><p><code>String</code></p>
</td>
<td><p>Makes this property part of a unicity scope. All tuples of properties
belonging to the same unicity scope are enforced to be unique in
the component type scope. This concretely translates to unique constraints
in the datastore spanning the properties composing the unicity
scope. Note that, for performance reasons, unicity scopes are only enforced
by the persistence layer.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>delegateWritable</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Instructs the framework that a delegate-computed property is
writable. Most of the time, a computed property is read-only. Whenever
a computed property is made writable through the use of
delegateWritable=true, the delegate class must also provide a
setter for the computed property.
Defult value is false.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>processors</strong></p><p><code>ListOfString</code></p><p><code>integrityProcessorClassNames</code></p>
</td>
<td><p>Registers a list of property processor instances that will be triggered
on the different phases of the property modification, i.e. :
<ul>
<li>before the property is modified, usually for controlling the incoming value
</li>
<li>while (actually just before the actual assignment) the property is
modified, allowing to intercept and change the incoming value
</li>
<li>after the property is modified, allowing to trigger some post-modification
behaviour (e.g. tracing, domain integrity management, ...)
</li>
</ul>
Property processor instances must implement the IPropertyProcessor&lt;
E, F&gt; interface where &lt;E, F&gt; represent respectively the
type of the owning component and the type of the property. Since
there are 3 methods to implement in the interface (1 for each of the
phase described above), Jspresso provides an adapter class with
empty implementations to override : EmptyPropertyProcessor&lt;E
, F&gt;.


Whenever the underlying property is a collection property, the interface
to implement is ICollectionPropertyProcessor&lt;E, F&gt; 
(or extend EmptyCollectionPropertyProcessor&lt;E, F&gt;) with
4 new phases introduced :
<ul>
<li>before an element is added to the collection property
</li>
<li>after an element is added to the collection property
</li>
<li>before an element is removed from the collection property
</li>
<li>after an element is removed from the collection property
</li>
</ul></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>initializationMapping</strong></p><p><code>Map</code></p>
</td>
<td><p>This property allows to pre-initialize UI filters that are based on this
reference property. This includes :
<ul>
<li>explicit filters that are dispayed for "list of values"
</li>
<li>implicit filters thet are use behind the scene for UI auto-completion
</li>
</ul>
The initialization mapping property is a Map keyed by referenced type
property names (the properties to be initialized).
Values in this map can be either :
<ul>
<li>a constant value. In that case, the filter property is initialize with this constant value.
</li>
<li>a owning component property name. In that case, the filter property 
is initialize with the value of the owning component property.
</li>
</ul></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>paramSets</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>paramSets makes it possible to use a list of paramSet. 
The properties declared in the paramSet come to be added to the properties of the current descriptor. 
The properties brought by the paramSet can be overridden  by the current descriptor. If a property is overridden with the null value the property is ignored.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>versionControl</strong></p><p><code>Boolean</code></p>
</td>
<td><p>This property allows to fine tune wether this component property participates
in optimistic versioning. It mainly allows to declare some
properties that should be ignored regarding optimistic versioning thus
lowering the risk of version conflicts between concurrent users.
Of course, this feature has to be used with care since it may generate
phantom updates to the data store.


Default value is true so that any change in the described property
increases the owning component version.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>computedFurther</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Forces a property to be considered as a computed property by the
framework. A computed property will be completely ignored by the
persistence layer and its management is left to the developer.


Properties declared with <b>computed = true</b> are considered
computed. However, there is sometimes a need to declare
a property at some level (e.g. in an interface descriptor) and let lower
level implementation decide how to handle this common property.
In that case, you can declare this property computedFurther=true
Default value is false</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>sortable</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Enforces a property sortability. This is only enforced at the UI level, i.e. the property can still be used
for sorting programmatically.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>cacheable</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Configures the fact that this property can be cached. This is only used for computed properties.
Note that the cached value will be reset whenever a firePropertyChange regarding this property
is detected to be fired.


Default value is false in order to prevent un-desired side-effects if computed property change
notification is not correctly wired.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>filterComparable</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Configures the fact that this property uses a comparator when installed n a filter view. By default,
number, date, time, and duration properties behave this way.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>BasicType
#### <a name=""></a>string


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicStringPropertyDescriptor`

Field Declaration of type String. To be used with _n where the value n determine the maximum string length 
<pre><b>string_32</b> 'name'

<b>string_10</b> 'zipCode'
</pre>


<table>
<caption>string properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>maxLength</strong></p><p><code>Integer</code></p>
</td>
<td><p>Configures the maximum string. Not needed with SJS which uses <b>string_n</b></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>regex</strong></p><p><code>Regexp</code></p><p><code>regexpPattern</code></p>
</td>
<td><p>regex pattern (regular expression) to be applied to validate the string
<pre>
string_128 'email', <b>regex</b>: "[\w\-\.]*@[\w\-\.]", 

       <b>regexSample</b>: 'contact@acme.com'
</pre></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>regexSample</strong></p><p><code>String</code></p><p><code>regexpPatternSample</code></p>
</td>
<td><p>Sample of a valid value for the regex pattern (see regex)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>upperCase</strong></p><p><code>Boolean</code></p>
</td>
<td><p>This is a shortcut to implement the common use-case of handling upper-case
only properties. all incoming values will be transformed to uppercase as if
a property processor was registered to perform the transformation.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>translatable</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Configures the string property to be translatable. Jspresso will then implement a translation store for the
property so that it can be displayed in the connected user language. This is particularly useful for
non-european character sets like chinese, indian, and so on. In that case, only the raw, untranslated
property value is stored in the object itself. the various translationsare stored in an extra store. translated
properties support searching, ordering,... exactly like non-translatable properties.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>defaultValue</strong></p><p><code>String</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>text


+ **extend** : `string`
+ **Inherited properties ** : `maxLength, regex, regexSample, upperCase, translatable, defaultValue, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicTextPropertyDescriptor`

Field declaration of type text. To be used with _n where the value n determine the maximum text length


<table>
<caption>text properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>fileFilter</strong></p><p><code>Map</code></p>
</td>
<td><p>This property allows to configure the file filter that has to be displayed
whenever a file system operation is initiated from the UI to operate on
this property. This includes :
<ul>
<li>setting the property value from a text file loaded from the file system
</li>
<li>saving the property text value to a file on the file system
</li>
</ul>
Jspresso provides built-in actions that do the above and configure
their UI automatically based on the fileFilter property.
The incoming Map must be structured like following :
<ul>
<li>keys are translation keys that will be translated by Jspresso i18n
layer and presented to the user as the group name of the associated
extensions, e.g. "HTML files"
</li>
<li>values are the extension list associated to a certain group name, e
.g. a list containing [".html",".htm"]
</li>
</ul></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>fileName</strong></p><p><code>String</code></p>
</td>
<td><p>Configures the default file name to use when downloading the property content as a file.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>contentType</strong></p><p><code>String</code></p>
</td>
<td><p>Configures the default content type to use when downloading the property content as a file.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>queryMultiline</strong></p><p><code>Boolean</code></p>
</td>
<td><p>This property allows to control if the text property view should be transformed into a multi-line
text view in order to allow for multi-line text in filters. Default value is false.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>imageUrl


+ **extend** : `string`
+ **Inherited properties ** : `maxLength, regex, regexSample, upperCase, translatable, defaultValue, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicImageUrlPropertyDescriptor`

Describes a property used for image URL values. This instructs Jspresso to display the property value as
an image instead of raw text content.


<table>
<caption>imageUrl properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>scaledWidth</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets scaled width. This property, when set to a positive integer will force the image width to be resized to the
target value. If only one of the 2 scaled dimensions is set, then the image is scaled by preserving its aspect
ratio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>scaledHeight</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets scaled height. This property, when set to a positive integer will force the image height to be resized to the
target value. If only one of the 2 scaled dimensions is set, then the image is scaled by preserving its aspect
ratio.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>password


+ **extend** : `string`
+ **Inherited properties ** : `maxLength, regex, regexSample, upperCase, translatable, defaultValue, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicPasswordPropertyDescriptor`

Describes a property used for password values. For obvious security reasons, this type of properties will
hardly be part of a persistent entity. However it is useful for defining transient view models, e.g. for implementing
a change password action. Jspresso will automatically adapt view fields accordingly, using password
fields, to interact with password properties.


<table>
<caption>password properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>integer


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicIntegerPropertyDescriptor`

Describes an integer property. The property is either represented as an
Integer or a Long depending on the property value.


<table>
<caption>integer properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>minValue</strong></p><p><code>Integer</code></p>
</td>
<td><p>Configures the upper bound of the allowed values. Default value is
null, meaning unbound</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>maxValue</strong></p><p><code>Integer</code></p>
</td>
<td><p>Configures the lower bound of the allowed values. Default value is
null, meaning unbound</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>usingLong</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Configures the property to be managed using java.lang.Long.
Default value is false which means java.lang.Integer will be used.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>thousandsGrouping</strong></p><p><code>Boolean</code></p><p><code>thousandsGroupingUsed</code></p>
</td>
<td><p>A boolean value indicating if the number should be formatted with thousands grouped</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>defaultValue</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>date


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicDatePropertyDescriptor`

Describes a date based property. Wether the date property should include time information or not, can be
configured using <b>date</b> or <b>date_time</b> declaration


<table>
<caption>date properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>type</strong></p><p><code>String</code></p>
</td>
<td><p>Configures if date property should include time information or not. Not needed with SJS which uses <b>date</b> or <b>date_time</b></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>timeZoneAware</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Sets wether this date property should have its string representation vary depending on the client timezone.


Default value is false, meaning that the date is considered as a string. It is in fact expressed in the server timezone.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>secondsAware</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Sets whether this date should include seconds information when configured as <b>date_time</b>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>format</strong></p><p><code>String</code></p><p><code>formatPattern</code></p>
</td>
<td><p>Configures a specific pattern to display this date</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>defaultValue</strong></p><p><code>String</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>bool


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicBooleanPropertyDescriptor`

Describes a boolean property


<table>
<caption>bool properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>defaultValue</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>decimal


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicDecimalPropertyDescriptor`

Describes a decimal property. Property value is either stored as a Double or as a BigDecimal depending
on the usingBigDecimal property


<table>
<caption>decimal properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>minValue</strong></p><p><code>BigDecimal</code></p>
</td>
<td><p>Configures the upper bound of the allowed values. Default value is
null, meaning unbound</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>maxValue</strong></p><p><code>BigDecimal</code></p>
</td>
<td><p>Configures the lower bound of the allowed values. Default value is
null, meaning unbound</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>usingBigDecimal</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Configures the property to be managed usin java.math.BigDecimal.
Default value is false which means java.lang.Double will
be used.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>maxFractionDigit</strong></p><p><code>Integer</code></p>
</td>
<td><p>Configures the precision of the decimal property. Default value is
null which means unlimited.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>thousandsGrouping</strong></p><p><code>Boolean</code></p><p><code>thousandsGroupingUsed</code></p>
</td>
<td><p>A boolean value indicating if the number should be formatted with thousands grouped</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>defaultValue</strong></p><p><code>BigDecimal</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>time


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicTimePropertyDescriptor`

Describes a property used to hold time only values. These properties use a Date to store their value but
only the time part of the value is relevant


<table>
<caption>time properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>secondsAware</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Sets whether this time should include seconds information.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>format</strong></p><p><code>String</code></p><p><code>formatPattern</code></p>
</td>
<td><p>Configures a specific pattern to display this time</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>defaultValue</strong></p><p><code>String</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>duration


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicDurationPropertyDescriptor`

Describes a property used to store a duration value. Duration is stored in the form of a number of milliseconds.
duration properties are cleanly handled by Jspresso UI layer for both displaying / editing duration
properties in a convenient human format.


<table>
<caption>duration properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>maxMillis</strong></p><p><code>Integer</code></p>
</td>
<td><p>Configures the maximum duration value this property accepts in milliseconds.
Default value is null, meaning unbound.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>defaultValue</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>percent


+ **extend** : `decimal`
+ **Inherited properties ** : `minValue, maxValue, usingBigDecimal, maxFractionDigit, thousandsGrouping, defaultValue, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicPercentPropertyDescriptor`

This is a specialization of decimal descriptor to handle percentage values. The impact of using this descriptor
is only on the UI level that will be configured accordingly, i.e. displaying/editing properties as percentage
instead of their raw decimal values.


<table>
<caption>percent properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>enumeration


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicEnumerationPropertyDescriptor`

descriptor for properties whose values are enumerated strings. An example of such a
property is gender whose value can be M (for "Male") or F (for "Female"). Actual property values can be
codes that are translated for inclusion in the UI. Such properties are usually rendered as combo-boxes.


<table>
<caption>enumeration properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>enumName</strong></p><p><code>String</code></p><p><code>enumerationName</code></p>
</td>
<td><p>This property allows to customize the i18n keys used to translate the
enumeration values, thus keeping the actual values shorter. For instance
consider the gender enumeration, composed of the M (for
"Male") and F (for "Female") values. Setting an enumeration name
to "GENDER" will instruct Jspresso to look for translations named
"GENDER_M" and "GENDER_F". This would allow for using M and
F in other enumeration domains with different semantics and translations.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>maxLength</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets the max size of the values.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>valuesAndIcons</strong></p><p><code>Map</code></p><p><code>valuesAndIconImageUrls</code></p>
</td>
<td><p>Defines the list of values as well as an icon image URL per value this
enumeration contains. The incoming Map is keyed by the actual enumeration
values and valued by the icon image URLs.
Enumeration values are translated in the UI using the following
scheme : [enumerationName]_[value].</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>values</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>Defines the list of values this enumeration contains.
Enumeration values are translated in the UI using the following
scheme : [enumerationName]_[value].</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>defaultValue</strong></p><p><code>String</code></p>
</td>
<td><p>Sets the property default value. When a component owning this property
is instanciated, its properties are initialized using their default values.
By default, a property default value is null.
This incoming value can be either the actual property default value
(as an Object) or its string representation whose parsing will be delegated
to the property descriptor.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>queryMultiselect</strong></p><p><code>Boolean</code></p>
</td>
<td><p>This property allows to control if the enumeration property view should be
transformed into a multi-selectable property view in order to allow for value
disjunctions in filters. Default value is false.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>typeEnumeration


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `TypeEnumerationPropertyDescriptor`

This is a special enumeration descriptor that allows to build the enumeration out of a list of component
descriptors. Enumeration values and icons are the names and icons of the registered component descriptors.
For instance, this can be useful in the UI if you want to visually indicate the actual type of a element
contained in a polymorphic collection


<table>
<caption>typeEnumeration properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>enumName</strong></p><p><code>String</code></p><p><code>enumerationName</code></p>
</td>
<td><p>This property allows to customize the i18n keys used to translate the
enumeration values, thus keeping the actual values shorter. For instance
consider the gender enumeration, composed of the M (for
"Male") and F (for "Female") values. Setting an enumeration name
to "GENDER" will instruct Jspresso to look for translations named
"GENDER_M" and "GENDER_F". This would allow for using M and
F in other enumeration domains with different semantics and translations.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>maxLength</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets the max size of the values.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>components</strong></p><p><code>ListOfRef</code></p><p><code>componentDescriptors</code></p>
</td>
<td><p>Registers the list of component descriptors to build the enumeration
values/icons from their names and icons.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>queryMultiselect</strong></p><p><code>Boolean</code></p>
</td>
<td><p>This property allows to control if the enumeration property view should be
transformed into a multi-selectable property view in order to allow for value
disjunctions in filters. Default value is false.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>range


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `RangeEnumerationPropertyDescriptor`

This is a special enumeration descriptor that allows to build the enumeration values out of a list of integer
values. Obviously, no icon is provided for a given value


<table>
<caption>range properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>rangeName</strong></p><p><code>String</code></p><p><code>enumerationName</code></p>
</td>
<td><p>This property allows to customize the i18n keys used to translate the
enumeration values, thus keeping the actual values shorter. For instance
consider the gender enumeration, composed of the M (for
"Male") and F (for "Female") values. Setting an enumeration name
to "GENDER" will instruct Jspresso to look for translations named
"GENDER_M" and "GENDER_F". This would allow for using M and
F in other enumeration domains with different semantics and translations.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>minValue</strong></p><p><code>Integer</code></p>
</td>
<td><p>The enumeration minimum bound. Default value is 0.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>maxValue</strong></p><p><code>Integer</code></p>
</td>
<td><p>The enumeration maximum bound. Default value is 10.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>rangeStep</strong></p><p><code>Integer</code></p>
</td>
<td><p>The step to use for constructing the enumeration values, starting
from minValue up to maxValue. Default value is 1.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>queryMultiselect</strong></p><p><code>Boolean</code></p>
</td>
<td><p>This property allows to control if the enumeration property view should be
transformed into a multi-selectable property view in order to allow for value
disjunctions in filters. Default value is false.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>refId


+ **mandatory** : `id`
+ **allowed previous element** : `Entity, Interface, Component`

allows to point on a reference


<table>
<caption>refId properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>id</strong></p><p><code>Ref</code></p>
</td>
<td><p>Qualifies the type of element this property refers to.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>color


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicColorPropertyDescriptor`

Describes a property used for storing a color. Color values are stored in the property as their string hexadecimal
representation (0xrgba encoded). Jspresso cleanly handles color properties in views for both visually
displaying and editing them without any extra effort. Moreover the ColorHelper helper class eases
colors manipulation and helps converting to/from their hexadecimal representation.


<table>
<caption>color properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>defaultValue</strong></p><p><code>String</code></p>
</td>
<td><p>Sets the default value for the field</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>binary


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicBinaryPropertyDescriptor`

Describes a property used to store a binary value in the form of a byte array


<table>
<caption>binary properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>maxLength</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets the max size of the byte array.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>fileFilter</strong></p><p><code>Map</code></p>
</td>
<td><p>This property allows to configure the file filter that has to be displayed
whenever a file system operation is initiated from the UI to operate on
this property. This includes :
<ul>
<li>setting the property binary value from a file loaded from the file system
</li>
<li>saving the property binary value to a file on the file system
</li>
</ul>
Jspresso provides built-in actions that do the above and configure
their UI automatically based on the fileFilter property.
The incoming Map must be structured like following :
<ul>
<li>keys are translation keys that will be translated by Jspresso i18n
layer and presented to the user as the group name of the associated
extensions, e.g. "JPEG images"
</li>
<li>values are the extension list associated to a certain group name, e
.g. a list containing [".jpeg",".jpg"]
</li>
</ul></p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>fileName</strong></p><p><code>String</code></p>
</td>
<td><p>Configures the default file name to use when downloading the property content as a file.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>contentType</strong></p><p><code>String</code></p>
</td>
<td><p>Configures the default content type to use when downloading the property content as a file.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>image


+ **extend** : `binary`
+ **Inherited properties ** : `maxLength, fileFilter, fileName, contentType, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicImageBinaryPropertyDescriptor`

Describes a property used for image binary values. This instructs Jspresso to display the property value as
an image instead of raw binary content.


<table>
<caption>image properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>scaledWidth</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets scaled width. This property, when set to a positive integer will force the image width to be resized to the
target value. If only one of the 2 scaled dimensions is set, then the image is scaled by preserving its aspect
ratio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>scaledHeight</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets scaled height. This property, when set to a positive integer will force the image height to be resized to the
target value. If only one of the 2 scaled dimensions is set, then the image is scaled by preserving its aspect
ratio.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>formatName</strong></p><p><code>String</code></p>
</td>
<td><p>Sets format name. When set to something not null (e.g. PNG, JPG, ...), the image is transformed to the specified
format before being stored.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>java


+ **extend** : `binary`
+ **Inherited properties ** : `fileFilter, fileName, contentType, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicJavaSerializablePropertyDescriptor`

Describes a property used to store any java Serializable object. The property value is serialized/deserialized
to/from the datastore. The operation is completely transparent to the developer, i.e. the developer
never plays with the serialized form.


<table>
<caption>java properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>maxLength</strong></p><p><code>Integer</code></p>
</td>
<td><p>Sets the max size of the object in Byte.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>any


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicObjectPropertyDescriptor`

This descriptore is used to describe an arbitrary object property for which the type can be
explicitely declared.


<table>
<caption>any properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>class</strong></p><p><code>String</code></p><p><code>modelTypeClassName</code></p>
</td>
<td><p>Configures the actual property type through its fully qualified name.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>sourcecode


+ **extend** : `text`
+ **Inherited properties ** : `fileFilter, fileName, contentType, queryMultiline, maxLength, regex, regexSample, upperCase, translatable, defaultValue, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicSourceCodePropertyDescriptor`

Describes a property as handing sourcecode content. This instructs Jspresso to display the property value
as sourcecode, using syntax coloring for instance, instead of displaying unformatted raw content. The
language used to format the property text content may be defined explicitely using the language property.


<table>
<caption>sourcecode properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>language</strong></p><p><code>String</code></p>
</td>
<td><p>Explicitely sets the language this sourcecode property should contain.
 This is only a hint fo Jspresso to configure the UI components accordingly to interact with this property.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>html


+ **extend** : `text`
+ **Inherited properties ** : `fileFilter, fileName, contentType, queryMultiline, maxLength, regex, regexSample, upperCase, translatable, defaultValue, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicHtmlPropertyDescriptor`

Describes a property as handing HTML content. This instructs Jspresso to display the property value as
HTML instead of raw text content.


<table>
<caption>html properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>Association
#### <a name=""></a>reference


+ **extend** : `common`
+ **Inherited properties ** : `description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **mandatory** : `ref`
+ **allowed previous element** : `Entity, Interface, Component`
+ **Jspresso** : `BasicReferencePropertyDescriptor`

This descriptor is used to describe a reference to an other component (entities, interfaces or components)


<table>
<caption>reference properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>ref</strong></p><p><code>Ref</code></p><p><code>referencedDescriptor</code></p>
</td>
<td><p>Qualifies the type of element this property refers to. It may point to
any type of component descriptor, i.e. entity, interface or component
descriptor.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>reverse</strong></p><p><code>Ref</code></p><p><code>reverseRelationEnd</code></p>
</td>
<td><p>Allows to make a relationship bi-directional. By default, when a relationdhip
end is defined, it is only navigable from the owning component
to the described end (default value is null). Assigning a reverse
relationship ends instructs the framework that the relationship
is bi-derectional. This implies several complementary features :
<ul>
<li>When one of the relationship ends is updated, the other
side is automatically maintained by Jspresso, i.e. you never
have to worry about reverse state. For instance, considering
a Invoice - InvoiceLine bi-directional relationship
, InvoiceLine.setInvoice(Invoice) and Invoice
.addToInvoiceLines(InvoiceLine) are strictly equivalent.
</li>
<li>You can qualify a "N-N" relationship (thus creating an association
table in the datastore behind the scene) by assigning 2 collection
property decriptors as reverse relation ends of each other.
</li>
<li>You can qualify a "1-1" relationship (thus enforcing some unicity
constraint in the datastore behind the scene) by assigning 2 reference
property decriptors as reverse relation ends of each other.
</li>
</ul>
Setting the reverse relation end operation is commmutative so that
it automatically assigns bot ends as reverse, i.e. you only have to set
the property on one side of the relationship.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>composition</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Instructs the framework that this property has to be treated as a composition,
in the UML terminology. This implies that reachable entities
that are referenced by this property follow the owning entity lifecycle. 
For instance, when the owning entity is deleted, the referenced entities
in composition properties are also deleted.
Whenever this property is not explicitely set by the developer, Jspresso
uses sensible defaults :
<ul>
<li>collection properties are compositions unless they are bidirectional
"N to N"
</li>
<li>reference properties are not composition
</li>
</ul>
This property is strictly behavioural and does not impact the domain
state itself.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>fetch</strong></p><p><code>String</code></p><p><code>fetchType</code></p>
</td>
<td><p>By default Jspresso apply a lazy loading stategy, by setting fetch=true
Jspresso will ask hibernate to load the child entity when parent is loaded.
This option has to be used carfully, if you define too many non-lazy associations in your object model,
Hibernate will fetch the entire database into memory in every transaction.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>batch</strong></p><p><code>Integer</code></p><p><code>batchSize</code></p>
</td>
<td><p>This property allows to finely tune batching strategy of the ORM on this relationship end. Whenever possible,
the ORM will use a IN clause in order to fetch multiple instances relationships at once. The batch size
determines the size of th IN clause.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>fkName</strong></p><p><code>String</code></p>
</td>
<td><p>Allows to customize the geneated foreign key (if any) name.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>rendered</strong></p><p><code>ListOfRefField</code></p><p><code>renderedProperties</code></p>
</td>
<td><p>This property allows to define which of the component properties are to be rendered by default when
displaying a list of value on this component family. For instance, a table will render 1 column per
rendered property of the component. Any type of property can be used except collection properties.
Since this is a List queriable properties are rendered in the same order.

Whenever this property is null (default value) Jspresso determines the default set of properties
to render based on the referenced component descriptor.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>queryable</strong></p><p><code>ListOfRefField</code></p><p><code>queryableProperties</code></p>
</td>
<td><p>This property allows to define which of the component properties are to be used in the list of value
filter that are based on this component family. Since this is a List queriable properties are rendered
in the same order.

Whenever this this property is null (default value), Jspresso chooses the default set of queryable
properties based on the referenced component descriptor.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>list


+ **extend** : `reference`
+ **Inherited properties ** : `ref, reverse, composition, fkName, rendered, queryable, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **mandatory** : `ref`
+ **allowed previous element** : `Entity, Interface, Component`

This descriptor is used to describe a collection of components (entities, interfaces or components).
A list allows for duplicates and preserves the order of the elements in the
datastore through an implicit index column.


<table>
<caption>list properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>ordering</strong></p><p><code>Map</code></p><p><code>orderingProperties</code></p>
</td>
<td><p>Ordering properties are used to sort this collection property if and only
if it is un-indexed (not a List). The sort order set on the collection
property can refine the default one that might have been set on the
referenced collection level. This property consist of a Map whose entries
are composed with :
<ul>
<li>the property name as key
</li>
<li>the sort order for this property as value. This is either a value of the
ESort enum (ASCENDING or DESCENDING) or its equivalent
string representation.
</li>
</ul>
Ordering properties are considered following their order in the map
iterator. A null value (default) will not give any indication for the collection
property sort order and thus, will delegate to higher specification
levels (i.e. the referenced collection sort order).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>manyToMany</strong></p><p><code>Boolean</code></p>
</td>
<td><p>Forces the collection property to be considered as a many to many (N-N) end. When a relationship is bi-directional,
setting both ends as being collection properties turns manyToMany=true automatically.
But when the relationship is not bi-directional, Jspresso has no mean to determine if the collection property
is 1-N or N-N. Setting this property allows to inform Jspresso about it.
Default value is false.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>fetch</strong></p><p><code>String</code></p><p><code>fetchType</code></p>
</td>
<td><p>By default Jspresso apply a lazy loading stategy, by setting fetch=true
Jspresso will ask hibernate to load the child entity when parent is loaded.
This option has to be used carfully, if you define too many non-lazy associations in your object model,
Hibernate will fetch the entire database into memory in every transaction.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>batch</strong></p><p><code>Integer</code></p><p><code>batchSize</code></p>
</td>
<td><p>This property allows to finely tune batching strategy of the ORM on this relationship end. Whenever possible,
the ORM will use a IN clause in order to fetch multiple instances relationships at once. The batch size
determines the size of th IN clause.</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>nullElement</strong></p><p><code>Boolean</code></p><p><code>nullElementAllowed</code></p>
</td>
<td><p>This property allows to declare if the collection property allows for null element, i.e. holes in lists.</p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>set


+ **extend** : `list`
+ **Inherited properties ** : `ordering, manyToMany, fetch, batch, nullElement, ref, reverse, composition, fkName, rendered, queryable, description, icon, iconWidth, iconHeight, i18nNameKey, mandatory, readOnly, preferredWidth, computed, sqlName, parent, id, grantedRoles, booleanWritabilityGates, rolesWritabilityGates, unicityScope, delegateWritable, processors, initializationMapping, paramSets, versionControl, computedFurther, sortable, cacheable, filterComparable`
+ **mandatory** : `ref`
+ **allowed previous element** : `Entity, Interface, Component`

This descriptor is used to describe a collection of components (entities, interfaces or components)..
A set do not allow for duplicates and do not preserve the order of the elements
in the datastore.


<table>
<caption>set properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>Support
#### <a name=""></a>external


+ **allowed previous element** : `Domain`

This descriptor allowed declaring references which are not described in the SJS description of the application.
since all references are controlled by SJS, it is necessary to declare the external references.


<table>
<caption>external properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="even">
<td align="left"><p><strong>id</strong></p><p><code>ListOfString</code></p>
</td>
<td><p>list identifiers of descriptors
<pre><b>external</b>(id: ['com.appli.model.content.aCatalog',

              'com.appli.model.content.aMenu'])</pre></p></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>paramSet


+ **allowed previous element** : `Domain, Entity, Interface, Component`

paramSet allows to create a reusable groups of properties in SJS declarations.
<pre>
paramSet 'myCommon', readOnly:true, mandatory:true 
</pre>
paramSet can be used by declaration SJS using the attribute paramSets


<table>
<caption>paramSet properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>namespace


+ **allowed previous element** : `Domain, Entity, Interface, Component`

namespace allows to declare and open a namespace scope. The use of namespaces allows simplifying the declarations SJS referring to resources with a complex path.
<pre>
namespace('org.jspresso.hrsample'){...}
</pre>
This declaration allows, for example, to replace the following statement
<pre>
Entity('City', 
 
        icon: 'classpath:org/jspresso/hrsample/images/city-48x48.png'){...}
</pre>
by
<pre>
('City',icon:'city-48x48.png') {...}
</pre>
With namespaces, conventions on the organization of the Jspresso's directories are used.
In this exemple, images are in the subdirectory /images of the project


<table>
<caption>namespace properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
#### <a name=""></a>include


+ **allowed previous element** : `Domain, Entity, Interface, Component`

include allows to use multi SJS sources files and to include them into each other.
<pre>
include('fileName')
</pre>


<table>
<caption>include properties</caption>
<colgroup>
<col width="33%" />
<col width="66%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Property</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">This class does not have any specific property.</td>
<td align="left"></td>
</tr>
</tbody>
</table>

---
