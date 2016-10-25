# Carbon UUF Maven Plugin

Carbon UUF Maven Plugin project provides a maven plugin for creating UUF Applications and UUF Components of the [Unified UI Framework(UUF)](https://github.com/wso2/carbon-uuf).

Carbon UUF Maven Plugin tries to reusing the existing maven plugins where as possible(i.e.Maven-Assembly-Plugin, Maven-Dependency-Plugin). This plugin provides two maven goals;

* create-component : This goal is used for creating UUF Component.
* create-app : This goal is used for creating UUF Application.
* create-theme : This goal is used for creating UUF Theme.

## Getting Started

A client maven module which needs to create a UUF application and/or component should add the plugin dependency into project pom.xml file.

#### 1) Creating a UUF Component

A component is a collection of reusable elements that offers application developers some functionality. A component may declare that it depends on other components. That is similar to an OSGi dependency - declaring that one component depends on another simply means that the dependent component will be available at runtime.

Project structure of UUF component

```xml
org.wso2.carbon.<product-name>.<feature-name>
|src
   |--main
        |---- pages
        |          |---- index.hbs
        |          |---- index.js
        |          |---- more-pages
        |          |              `one-more-page.hbs
        |          `---- my-page.hbs
        |  
        |---- public
        |          |---- css
        |          |---- js
        |          |---- lib
        |          `---- images
        |
        |---- layout
        |          |---- one-column.hbs
        |          `---- two-column.hbs
        |
        |---- fragments
        |          `---- listing
        |                |---- listing.hbs
        |                |---- listing.js
        |                |---- listing.json
        |                `---- public
        |                         |---- css
        |                         |---- js /* front-end JS */
        |                         |---- images /* images */
        |                         `---- lib /* front-end libs */
        |
|---- pom.xml /* defines dependent components */
```
Please refer complete version of simple-oauth component sample [here](https://github.com/wso2/carbon-uuf/tree/master/samples/components/org.wso2.carbon.uuf.sample.simple-auth)

User should add the plugin dependency into project pom.xml file in order to create an UUF component as follows. 

```xml
<build>
   <plugins>
       <plugin>
           <groupId>org.wso2.carbon.uuf.maven</groupId>
           <artifactId>carbon-uuf-maven-plugin</artifactId>
           <version>1.0.0-m5</version>
           <extensions>true</extensions>
           <executions>
               <execution>
                   <phase>package</phase>
                   <goals>
                       <goal>create-component</goal>
                   </goals>
                   <configuration>
                       <instructions>
                           <!-- import necessary packages -->
                           <Import-Package> … </Import-Package>
                       </instructions>
                   </configuration>
               </execution>
           </executions>
       </plugin>
   </plugins>
</build>
```
Once you add the carbon-uuf-maven-plugin and set the goal as ‘create-component’, you can create a uuf component by running maven packaging command as ‘mvn clean install’.  You should import external packages you used inside the component as mentioned in the above pom file. 

(e.g : <Import-Package> org.wso2.carbon.uuf.sample.simpleauth.bundle </Import-Package>)

#### 2) Creating a UUF Theme

A theme is a named collection of stylesheets, supporting images & fonts. Themes are shareable across applications by the application declaring the themes it uses. 

Each application must design its own mechanism to allow the user to switch themes. UUF will provide an API (available on the server-side) to select a theme. By default the "default" theme will be used to render the application.

Project structure of UUF theme
```xml
org.wso2.carbon.theme.<theme-name>/
   |src
      |--main
          |---- public/
          |      |---- css/
          |      |---- js/
          |      |---- images/
          |      `---- libs/
          |---- theme.yaml
   |---- pom.xml
```

Please refer complete version of ‘green’ theme sample [here](https://github.com/wso2/carbon-uuf/tree/master/samples/themes/org.wso2.carbon.uuf.sample.theme.green).

User should add the plugin dependency into project pom.xml file in order to create an UUF theme as follows. 

```xml
<build>
   <plugins>
       <plugin>
           <groupId>org.wso2.carbon.uuf.maven</groupId>
           <artifactId>carbon-uuf-maven-plugin</artifactId>
           <version>1.0.0-m5</version>
           <extensions>true</extensions>
           <executions>
               <execution>
                   <phase>package</phase>
                   <goals>
                       <goal>create-theme</goal>
                   </goals>
               </execution>
           </executions>
       </plugin>
   </plugins>
</build>
```
Once you created a project according as described in the above. You can create a UUF theme by running ‘mvn clean install’. Please not that goal is set to 'create-theme'

#### 3) Creating a UUF Application

An application is simply a component that is taking over "/" in a web context and also defines which other components are to be incorporated below "/" with their own sub-contexts. It also indicates the themes it wishes to include into the application.

This is the way of creating the UUF application. All the `Pages` and `Fragments` of the current Application will be moved into a component called "root" inside the "/components" folder.
  
Project structure of an app
```xml
org.wso2.carbon.<product-name>.<app-name>
   |--src
     |--main
        |---- pages/ - page structure of UUF application
        |---- fragments/
        |---- layouts/
        |---- modules/
        |---- lang/  - include language prop file ex: en_US.properties
        |---- public/
        |          |---- css /* CSS files only */
        |          |---- js  /* front-end JS */
        |          |---- images  /* images */
        |          |---- lib /* other front-end libraries e.g. D3 */
   |---- pom.xml /* defines dependent components */
```

Please refer complete version of pets-store sample [here](https://github.com/wso2/carbon-uuf/tree/master/samples/apps/org.wso2.carbon.uuf.sample.pets-store)

User should add the plugin dependency into project pom.xml file in order to create an UUF application as follows. 

```xml
<build>
   <plugins>
       <plugin>
           <groupId>org.wso2.carbon.uuf.maven</groupId>
           <artifactId>carbon-uuf-maven-plugin</artifactId>
           <version>1.0.0-m5</version>
           <extensions>true</extensions>
           <executions>
               <execution>
                   <id>create</id>
                   <phase>package</phase>
                   <goals>
                       <goal>create-app</goal>
                   </goals>
                   <configuration>
                       <instructions>
                           <!-- import necessary packages -->
                           <Import-Package> … </Import-Package>
                       </instructions>
                       <!--All backend dependencies should be specified here-->
                       <bundles>
                           <bundle>
                               <symbolicName> … </symbolicName>
                               <version> … </version>
                           </bundle>
                       </bundles>
                   </configuration>
               </execution>
           </executions>
       </plugin>
   </plugins>
</build>
```
Since we are going to create a UUF application, the goal should be defined as ‘create-app’. Moreover the external packages that we used in this application should be imported as in the above pom file.
```xml
(e.g. : - <Import-Package> org.wso2.carbon.uuf.sample.simpleauth.bundle </Import-Package> ). 
```
Moreover all backend dependencies should be specified as following example.

 e.g :
 ```xml
<bundle>
    <symbolicName>org.wso2.carbon.uuf.sample.pets-store.bundle</symbolicName>
    <version> 1.0.0 </version>
</bundle>
```
Once you created a UUF application according to above mentioned project structure, you can package it using maven build tool using ‘mvn clean install’. After you building the uuf application, it can be treated as carbon feature and can be installed into carbon kernel as a feature. 

#### 4) Adding UUF Components and Themes dependencies to the UUF Application.

Following UUF Application reuses the UUF components "base"(org.wso2.carbon.uuf.base) and "basicauth"(org.wso2.is.uuf.basicauth) and utilize the theme "dark"(org.wso2.uuf.theme.dark).

```xml
<dependencies>
   <!-- themes -->
   <dependency>
      <groupId>org.wso2.uuf</groupId>
      <artifactId>org.wso2.uuf.theme.dark</artifactId>
      <version>1.0.0-SNAPSHOT</version>
      <type>zip</type>
  </dependency>
  <!-- components -->
  <dependency>
      <groupId>org.wso2.uuf</groupId>
      <artifactId>org.wso2.uuf.base</artifactId>
      <version>1.0.0-SNAPSHOT</version>
      <type>zip</type>
  </dependency>
  <dependency>
      <groupId>org.wso2.is</groupId>
      <artifactId>org.wso2.is.uuf.basicauth</artifactId>
      <version>1.0.0-SNAPSHOT</version>
      <type>zip</type>
  </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.wso2.carbon.uuf.maven</groupId>
            <artifactId>carbon-uuf-maven-plugin</artifactId>
            <version>1.0.0-m5</version>
            <executions>
                <execution>
                    <id>create</id>
                    <phase>package</phase>
                    <goals>
                        <goal>create-app</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

#### 5) When a UUF Application depends on another UUF Application.

Following UUF Application reuses the UUF Application "pets-store"(org.wso2.carbon.uuf.sample.pets-store). In this case, the "root" components of the both applications are merged. When a duplicate occurs target application receives the priority.

```xml
<dependencies>
    <dependency>
        <groupId>org.wso2.uuf.sample</groupId>
        <artifactId>org.wso2.uuf.sample.pets-store</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <type>zip</type>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.wso2.carbon.uuf.maven</groupId>
            <artifactId>carbon-uuf-maven-plugin</artifactId>
            <version>1.0.0-SNAPSHOT</version>
            <executions>
                <execution>
                    <id>create</id>
                    <phase>package</phase>
                    <goals>
                        <goal>create-app</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```
#### OSGi Imports for UUF Artifacts
If you are using Java classes exported by other OSGi bundles inside your JavaScript files, you need to explicitly mention the package imports inorder to minimize classloading complexisities. For instance;

```xml
<properties>
    <import.package>
        org.wso2.carbon.uuf.*;version=[1.0.0,2.0.0],
        org.wso2.msf4j
    </import.package>
</properties>
```
