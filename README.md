# API_SOAPUI_GrovvyScripting

//import classes
import com.eviware.soapui.support.XmlHolder
import com.eviware.soapui.impl.wsdl.testcase.WsdlTestRunContext

//log.info to print
log.info "this is my first code"

//context.expand to read the property in same test case
log.info context.expand ('${#TestCase#name}')
log.info context.expand ('${#TestCase#Age}')

//testrunner to red script from anywhere in the project
log.info testRunner.testCase.testSuite.testCases['AddEmployee'].getPropertyValue("name")
log.info testRunner.testCase.testSuite.testCases['GetEmployee'].getPropertyValue("department")
log.info testRunner.testCase.getPropertyValue("name")
//read from project level
def pp =testRunner.testCase.testSuite.project
log.info pp.getPropertyValue("department")

//Automate Add employee

//Add variable addReq to get to the Request propetry to read the request details
def addReq = testRunner.testCase.testSuite.testCases['AddEmployee'].testSteps['add'].getPropertyValue("Request") 
//read name,age and id from properties
def name=testRunner.testCase.testSuite.testCases['AddEmployee'].getPropertyValue("name") 
def id=testRunner.testCase.testSuite.testCases['AddEmployee'].getPropertyValue("id") 
def age=testRunner.testCase.testSuite.testCases['AddEmployee'].getPropertyValue("age") 
//Create XMLholder to read the above xml addreq
def XmlAdd= new XmlHolder(addReq)

//update the mentioned values in the XML
XmlAdd.setNodeValue("//typ:addEmployee/typ:name",name)
XmlAdd.setNodeValue("//typ:addEmployee/typ:id",id)
XmlAdd.setNodeValue("//typ:addEmployee/typ:Department","cse")
XmlAdd.setNodeValue("//typ:addEmployee/typ:age",age)

//update the request property with new XML
def newAddXml=XmlAdd.getXml();
testRunner.testCase.testSuite.testCases['AddEmployee'].testSteps['add'].setPropertyValue("Request",newAddXml)
//log.info newAddXml

//Call the service i.e. run the request automatically
def addTestStep=testRunner.testCase.testSuite.testCases["AddEmployee"].testSteps["add"]
def contextAddEmployee= new WsdlTestRunContext(addTestStep); 
addTestStep.run(testRunner,contextAddEmployee)
