
# Dynamic Forms - Overview

> If you ever need an explanation that you can't find in this
> documentation, you can email Kevin Aud at kevinaud@gmail.com or text /
> call at 540-905-9197

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
		 - The Dynamic Form Template Editor app uses [Vuex Modules](https://vuex.vuejs.org/guide/modules.html) to split up the state management for the different pieces of the template (Question Groups, Questions, Question Choices, Rules, Actions, etc.)
		 - The Dynamic Form Instance app uses a single file to define the Store (since there is far less state to manage in this app)
	 - 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE3NTkzMjY1NDMsLTI1MTUyNjA5OSwtMj
gyMTUxNDI2LC04OTU4Mzc1NTksMTQ4NzgxNTMyOF19
-->