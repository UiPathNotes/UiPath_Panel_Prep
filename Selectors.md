# Selectors
## What are Selectors?
A Selector in UiPath Studio is a feature that enables the **identification** of the User Interface elements through its **address and attributes** stored as XML fragments. The element identification is done to perform specific activities in an automation project. Selectors are generated automatically every time we use an activity that interacts with graphical user interface elements.

We can think of the **element identification process** achieved through Selectors as a postman that delivers letters to a certain address. In order for the postman to deliver the letters, a specific path is required and must contain **structured and hierarchized details** such as Country > City > Zip Code > Street Name > Street Number > Apartment Number. Similarly, UiPath Studio requires the detailed path to a specific element within the user interface.

## The Structure of Selectors
User interfaces (UIs) are built using a series of containers nested one inside the other. Let’s take the example of a selector for the First name input field in MyCRM Application, and try to understand the meaning of the structure.

## Tags & Attributes of Selectors
As you saw, selectors are made of nodes. And each node is made of tags and attributes. Let's take an example to explain the two. Below is a selector node.

### Tags
- Nodes in the selector XML fragment
- Correspond to a visual element on the screen
- First node is the app window
- Last node is the element itself

For example:
- **wnd** (window)
- **html** (web page)
- **ctrl** (control)
- **webctrl** (web page control)
- **java** (Java application control)

### Attributes
Every attribute has a name and a value. You should use only attributes with constant or known values.

For example:
- parentid=‘slide-list-container’
- tag=‘A’
- aaname=‘Details’
- class=‘btn-dwnl’

## Types of Selectors
As previously presented, selectors are automatically generated when UI elements are indicated inside activities or when the recorder is used. Knowing the difference between full and partial selectors is very important when using activities generated or added inside containers outside the containers, or the other way around.

The containers in UiPath are Attach Window, Attach Browser and Open Browser.

### Full Selectors
- Contain all the tags and attributes needed to identify a UI element, including the top-level window
- Generated by the Basic Recorder
- Best suited when the actions performed require switching between multiple windows.

### Partial Selectors
- Don’t contain the tags and attributes of the top-level window, thus the activities with partial selectors must be enclosed in containers
- Generated by the Desktop Recorder
- Best suited for performing multiple actions in the same window.

## When are partial or full selectors used?
The best example of using a **Partial Selector** would be a simple automation where the deployed workflow only performs actions in the **same application** without shifting through multiple windows like a simple CRM.
On the other hand if the workflow would actually be required to interact with **multiple windows** like the same **CRM and a document**, which would make the UI elements required in this particular example dispersed in **multiple windows**, a **Full Selector** would be required.

## Fine-tuning

### When is fine-tuning of selectors required?
- **Dynamically generated selectors** - As it happens with some websites, the values of the attributes change with each visit.
- **Selectors being too specific** - Some selectors are automatically generated with the name of the file or with a value that changes. Here, placeholders are very useful.
- **System changes** - Some selectors contain the version of the application or another element that changes when the application is updated.
- **Selectors using IDX** - The IDX is the index of the current element in a container with multiple similar elements. This might change when a new element appears in the same container.


### What is fine-tuning?
Fine-tuning is the process of refining selectors in order to have the workflow correctly executed in situations in which the generated selector is **unreliable, too specific** or **too sensitive** with regards to system changes.

It mainly consists of small simple changes that have a larger impact on the overall process, such as adding **wildcards**, using the **repair** function or using **variables** in selectors.

## Managing Difficult Situations
In most of the cases in which the selectors automatically generated are not reliable enough, fine-tuning will solve the issue. However, there are some other situations, that we call difficult. Consider the example of a UI element that changes state, position or ID every time the workflow is run.

For these, there are other approaches:
- **Anchor Base** - This is very useful in cases in which the attribute values are not reliable (are generated at each execution, for example, but there is a UI element that is stable and is linked to the target UI element. The Anchor Base activity has two parts, one to locate the anchor UI element (like ‘Find Element’), and the second to perform the desired activity
- **Relative Selector** - This activity will basically incorporate the information about the anchor’s selector in the selector of the target UI element. However, the new selector will probably need additional editing, as some nodes of the first selector will still be in the new one. The solution is to have that part (like a dynamic ID) removed, and the selector will stabilize using the anchor’s selector.
- **Visual Tree Hierarchy** - The hierarchy in the Visual Tree can improve the reliability of a selector by including the tags and attributes of the element that is above in the hierarchy. This is very useful when the target UI element’s selector is not reliable, but the selector of the UI element right above in the hierarchy is. However, again, the selector needs further editing and validation, as the dynamic part needs to be removed and, at the same time, you need to make sure that the target element can be identified with a unique attribute.
- **Find Children** - This activity can identify all the children of an element that is more stable. Since its output is the collection of children, you will need to come up with a mechanism to identify only the target UI element (using one of its attributes, that makes is unique between the children, but wouldn’t be enough to identify it universally).