# Uibasics
UI5 basics

//View.xml

<core:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc"
	xmlns:ca="sap.ca.ui" xmlns:f="sap.ui.layout.form" xmlns:comm="sap.ui.commons"
	xmlns:l="sap.ui.layout" xmlns="sap.m"
	controllerName="com.peol.firstProject.view.CrmLeads"
	xmlns:html="http://www.w3.org/1999/xhtml">
	<Page id="ID_PG_DETAIL_PAGE" title="{i18n>crmLeads}"
		enableScrolling="false" showNavButton="true" class="sapUiSizeCompact"
		navButtonPress="handleBack">
		<content>
			<Button type="default" press="onOpen" text="Open Form"></Button>
		
			<VBox class="sapUiSmallMargin">
		<f:SimpleForm id="SimpleFormChange354"
			editable="true"
			layout="ResponsiveGridLayout"
			title="Employee Form"
			labelSpanXL="3"
			labelSpanL="3"
			labelSpanM="3"
			labelSpanS="12"
			adjustLabelSpan="false"
			emptySpanXL="4"
			emptySpanL="4"
			emptySpanM="4"
			emptySpanS="0"
			columnsXL="1"
			columnsL="1"
			columnsM="1"
			singleContainerFullSize="false" >
			<f:content>
				<Label text="Name" />
				<Input id="name" value="{/name}" />
				
				<Label text="Emp No." />
				<Input id="empno" value="{/eno}"/>
				
				
				<Label text="Address" />
				<TextArea id="add" value="{/add}" />
				
				<Label text="Contact No" />
				<Input id="number" value="{/number}" />
				
				
				<Label text="Email" />
				<Input id="email" value="{/email}" />
				
				<Label></Label>
				
				
				
				
			
						
	            <Button text="Save" type="Accept" press="onSave" /> 
	            <Button text="Clear" type="Reject"  press="onClear" />
	            
	
				
			</f:content>
		</f:SimpleForm>
		<Table
title = "EmpLoye"
id="EmpDetails" items="{/results}">
<columns>
    <Column width="11rem">
    	<Text text="Name"/>
    </Column>
     <Column width="11rem">
     	<Text text="Empno"/>
     </Column>
	<Column width="11rem">
 		<Text text="Address"/>
	</Column>
	<Column width="11rem">
		<Text text="Number"/>
	</Column>
	<Column width="11rem">
		<Text text="Email Id"/>
	</Column>

	<Column width="11rem">
		<Text text="Action"/>
	</Column>

</columns>
<items>
<ColumnListItem>
<cells>
<Text text="{name}"/>
<Text text="{eno}"/>
<Text text="{add}"/>
<Text text="{number}"/>
<Text text="{email}"/>

<FlexBox>
   <Button icon="sap-icon://edit" press="onEdit"></Button>
    <Button icon="sap-icon://delete" press="onDelete"></Button>
 </FlexBox>
   
</cells>
</ColumnListItem>
</items>
</Table>
		
	</VBox>
		
			   
		</content>
		<footer>
			<Toolbar id="ID_FOOTER">
				<ToolbarSpacer />
			</Toolbar>
		</footer>
	</Page>
</core:View>


//Controller.js


jQuery.sap.require("sap.m.MessageBox");
jQuery.sap.require("com.peol.firstProject.util.Formatter");
sap.ui.controller("com.peol.firstProject.view.CrmLeads", {
	onInit : function() {
		EmpDetails=[];
		// alert("onInit function called");
		
		var form_data={
				name : "",
				eno : "",
				add : "",
				number : "",
				email : ""
		};
var formModel = new sap.ui.model.json.JSONModel(form_data);
		
		this.getView().byId("SimpleFormChange354").setModel(formModel);
		
	},

	onBeforeRendering : function() {
		// alert("onBeforeRendering function called");
	},

	onAfterRendering : function() {
		// alert("onAfterRendering function called");
	},

	onExit : function() {
		// alert("onExit function called");
	},

	
onSave : function(){
	
	/*var Name = this.byId("name").getValue();
	if(Name.trim() === ""){
		this.byId("name").setValueState("Error");
	}
	else{
		//MessageBox.information("Input element contain text" +Name);
		this.byId("name").setValueState("none");
	}*/
	
	var ename = this.getView().byId("name").getValue();
	var eno = this.getView().byId("empno").getValue();
	var eadd = this.getView().byId("add").getValue();
	var enu = this.getView().byId("number").getValue();
	var eemail = this.getView().byId("email").getValue();
	
	var EmpLoye = {
			name : ename,
			eno : eno,
			add : eadd,
			number : enu,
			email : eemail
	};
	
	
	if(this.editIndicator == undefined){
		EmpDetails.push(EmpLoye);
	}
	else
		{
		EmpDetails[this.editIndicator] = EmpLoye;
		}

			
		    var ata = {results: EmpDetails};
			
			var tabmodel = new sap.ui.model.json.JSONModel(ata);
			this.getView().byId("EmpDetails").setModel(tabmodel);
		
	},
	
	//Clear
	onClear : function()
	{
		this.getView().byId("name").setValue("");
		this.getView().byId("empno").setValue("");
		this.getView().byId("add").setValue("");
		this.getView().byId("number").setValue("");
		this.getView().byId("email").setValue("");
		
	},
	//Edit
	onEdit : function(evt){
		console.log(evt);
		var eindex = parseInt(evt.mParameters.id.substr(-1));
		console.log(eindex);

		var abta = EmpDetails[eindex];
		this.editIndicator = eindex;
		
		var formModel = new sap.ui.model.json.JSONModel(abta);
		
		this.getView().byId("SimpleFormChange354").setModel(formModel);
		
	},
	
	
	//Delete
	onDelete : function(evt){
		console.log(evt);
		var eindex = parseInt(evt.mParameters.id.substr(-1));
		delete EmpDetails[eindex];
		
		EmpDetails = EmpDetails.filter(function(element){
			return element !== undefined;
		});
		
	 var  abta = {results : EmpDetails};
	 var tabmodel = new sap.ui.model.json.JSONModel(abta);
	 this.getView().byId("EmpDetails").setModel(tabmodel);
	 this.editIndicator = undefined;
	},
	
	onOpen : function(){
			if(!this.dialog){
			this.dialog = sap.ui.xmlfragment("com.peol.firstProject.fragments.myform", this);
			this.getView().addDependent(this.dialog);
			}
			this.dialog.open();
			
		},
		onClose : function(){
			 this.dialog.close();
	},
	
	onADD : function(){
		
		var ename = sap.ui.getCore().byId("name").getValue();
		var eno = sap.ui.getCore().byId("empno").getValue();
		var eadd = sap.ui.getCore().byId("add").getValue();
		var enu = sap.ui.getCore().byId("number").getValue();
		var eemail = sap.ui.getCore().byId("email").getValue();
		
		var EmpLoye = {
				name : ename,
				eno : eno,
				add : eadd,
				number : enu,
				email : eemail
		};
		
		
		EmpDetails.push(EmpLoye);

				
			    var ata = {results: EmpDetails};
				
				var tabmodel = new sap.ui.model.json.JSONModel(ata);
				this.getView().byId("EmpDetails").setModel(tabmodel);
				this.editIndicator = undefined;
		
	}
	
  
	
	

});







//Fragmentation

<core:FragmentDefinition
    xmlns="sap.m"
    xmlns:l="sap.ui.layout"
    xmlns:f="sap.ui.layout.form"
    xmlns:core="sap.ui.core">
 
    <Dialog title = "INFORMATION">
    <l:Grid defaultSpan = "L12 M12 S12" width = "auto" id = "idGrid"></l:Grid>
    <VBox class="sapUiSmallMargin">
		<f:SimpleForm id="SimpleFormChange354"
			editable="true"
			layout="ResponsiveGridLayout"
			title="Employee Form"
			labelSpanXL="3"
			labelSpanL="3"
			labelSpanM="3"
			labelSpanS="12"
			adjustLabelSpan="false"
			emptySpanXL="4"
			emptySpanL="4"
			emptySpanM="4"
			emptySpanS="0"
			columnsXL="1"
			columnsL="1"
			columnsM="1"
			singleContainerFullSize="false" >
			<f:content>
				<Label text="Name" />
				<Input id="name"  />
				
				<Label text="Emp No." />
				<Input id="empno" />
				
				
				<Label text="Address" />
				<TextArea id="add"  />
				
				<Label text="Contact No" />
				<Input id="number" />
				
				
				<Label text="Email" />
				<Input id="email" />
				
				<Label></Label>
				
				
				
				
			
						
	            <Button text="ADD" type="Accept" press="onADD" /> 
	            <Button text="Close" type="Reject"  press="onClose" />
	            
	
				
			</f:content>
		</f:SimpleForm>
		</VBox>
		</Dialog>
		</core:FragmentDefinition>
