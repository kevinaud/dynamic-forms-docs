
# Dynamic Forms - Overview

> If you ever need an explanation that you can't find in this
> documentation, you can email Kevin Aud at kevinaud@gmail.com or text /
> call at 540-905-9197

## Back End
 - DynamicFormTemplate
	 - Each type of assessment is associated with one DynamicFormTemplate per organization. 
	 - For example, Tarrant County has exactly one DynamicFormTemplate for the Custody Level Assessment and Dallas county has exactly one DynamicFormTemplate for the Custody Level Assessment. This allows Tarrant County to configure the Custody Level Assessment without it affecting Dallas County.
- DynamicFormInstance
	- This is the domain model that gets associated with a BookIn. It is created based off of a DynamicFormTemplate when an assessment is submitted.
- Polymorphism
	- Many of the domain models for Dynamic Forms take advantage of [NHibernate's support for Inheritance Mappings](http://nhibernate.info/doc/nhibernate-reference/inheritance.html)
	- There are multiple inheritance mapping strategies supported by NHibernate, the one I used is Table per Class Hierarchy
	- The Domain Models this applies to (at the time this was written) are:
		- `DynamicFormQuestion`, an abstract class extended by
			- `DynamicFormMultipleChoiceQuestion`
			- `DynamicFormTextQuestion`
			- `DynamicFormNumericQuestion`
			- `DynamicFormDecimalQuestion`
		- `FormEffect`, an abstract class extended by
			- `HideQuestionEffect`
			- `ShowQuestoinEffect`
		- `FormAction`, an abstract class extended by
			- `SetCustodyLevelAction`
		- `FormRule`, an abstract class extended by
			- `AlwaysTrueFormRule`
			- `GroupPointsComparisonFormRule`
			- `TotalPointsComparisonFormRule`
			- `CompositeFormRule`
			- `IsNotSelectedFormRule`
			- `IsSelectedFormRule`
	- I designed it this way to make it easy to add new functionality to Dynamic Form Templates. Instead of the DynamicFormTemplate class having a list of Multiple Choice Questions **and** a list of Text Questions **and** a list of Numeric Questions, etc. it simply has a list of Questions. It doesn't care what kind of questions are in the list, it doesn't care if a new type of question is added, it just treats them all as questions.
	- Logic that is different for each subclass
		- There are some scenarios where you can't just treat everything like a Question. Sometimes it matters if it is a Multiple Choice Question or a Text Question.
		- One example is when you need to take a list of Questions and convert them into their respective view models. The view model for a Text Question is different than the view model for a Multiple Choice Question because there are some properties that exist on a Multiple Choice Question and not on a Text Question (like a list of Choices).
		- The wrong way to solve that problem:
    `foreach (var question in questions)`
    `if (question is )`
  ``
    ``

    

## Front End

 - The front end consists of two VueJS applications
	 - Dynamic Form Template Editor
		 - Source code folder: `UI/Features/DynamicForms/vue/`
		 - Bundled Output: `UI/Content/js/DynamicForms.App.js`
		 - This application is responsible for editing a Dynamic Form Template
	- Dynamic Form Instance
		 - Source code folder: `UI/Features/DynamicFormInstance/vue/`
		 - Bundled Output: `UI/Content/js/DynamicFormInstance.App.js`
		 - This application is responsible for rendering a Template in the form of an assessment that can be filled out, submitted, and attached to a book in
 - State Management
	 - Both VueJS applications use [Vuex](https://vuex.vuejs.org/) (VueJS implementation of redux) for state management
	 - The Dynamic Form Template Editor app uses [Vuex Modules](https://vuex.vuejs.org/guide/modules.html) to split up the state management for the different pieces of the template (Question Groups, Questions, Question Choices, Rules, Actions, etc.). The modules can be found at `UI/Features/DynamicForms/vue/store/modules/`
	 - The Dynamic Form Instance app uses a single file to define the Store, since there is far less state to manage in this app, but in the future it might make sense to split it up into modules as well. The store file can be found at `UI/Features/DynamicFormInstance/vue/store.js`
	 - Normalized Data
		 - For both the Template Editor app and Instance Rendering app I decided to "Normalize" the state data. This is considered a best practice when using redux and, although it looks strange at first, it makes it much easier to work with nested/relational state.
		 - I strongly suggest you read [this article](https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape) if you are trying to understand / change / add to the Store in either of the VueJS apps. I used that article as a reference when structuring the data so the way that I structured it will make more sense after you've read it.
- Layout
	 - In the Instance Renderer app I mostly used [Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) for the layout
	 - In the Template Editor I made heavy use of a new CSS feature called [CSS Grid](https://css-tricks.com/snippets/css/complete-guide-grid/) for the layout 
		 - Most of the various grid configurations are defined at the bottom of `App.vue`

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQxMDExMDc3NCwtMjAyMzMwNzQxNCwxMz
kxODI2NjA1LDE4NTI5MDcxNzMsMjA4ODMyNDEzNCwtMTIxNTU3
MzM5NSw1Mzg1NzM0NzgsLTI1MTUyNjA5OSwtMjgyMTUxNDI2LC
04OTU4Mzc1NTksMTQ4NzgxNTMyOF19
-->