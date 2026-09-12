
The IoC manage by framework all things manage by spring framework --  we called Bean class  

**Learn : .jar file && .compile file**  

MAVEN is a dependency manager we have to talk to it - it will get a all dependecy download for us as per we said, lets say if i say I depend on spring then it will download all dependecy required.  

<img width="519" height="193" alt="image" src="https://github.com/user-attachments/assets/baa3ee7a-f2a6-41c3-954e-be63f9dbf8d9" />

Here we talk to maven with the pom.xml file and languague we used to talk to maven is readily available we just have to go to 1) Maven Repository (MVN repo) site 2) Search whatever you want go the respecitive version you want click there you can seee the code below for it.  3) Simple then we need to create `<dependecies></dependecies>` tah in pom.xml and inside that tag we need to paste it. 4)  then we need to call maven so maven can read from what we have entered and import or download the necessary things (Right click on project >> maven >> update the project) *You can see one folder will automatically added Maven dependecies. 

<img width="487" height="286" alt="image" src="https://github.com/user-attachments/assets/06c257ca-c210-4659-9cd7-009ca5e9e68b" />

When ever we create the java maven the default version created `1.5` if need to change manaully we need to change it

<img width="659" height="475" alt="image" src="https://github.com/user-attachments/assets/dbf2d434-dbd4-4c7b-bd4b-e0a8c08882be" />

<img width="941" height="454" alt="image" src="https://github.com/user-attachments/assets/54a92e1f-ad09-485a-a06a-d7d2ba24fe8a" />

Bean factory -- 
`DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();`  


