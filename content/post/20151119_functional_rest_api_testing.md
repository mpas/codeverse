+++
title = "Functional Rest API Testing with Grails/Rest Client Builder"
date = "2015-11-19"
categories = ["Development"]
tags = ["grails", "spock", "rest", "testing"]
+++

Functional Rest API testing with Grails is easy and fun. We will be creating a simple Rest Controller and test it using Spock and Rest Client Builder.

When running the functional test a real container will be started on a specific port and tests are run against the running container. Just as it should be.

**Scenario:**<br>
Performing a GET request on a url (http://localhost:8080/helloworld) should return a `HTTP Status 200` and data with a json payload `{"message":"helloworld"}`

So lets get started!

**Create a Grails application**

    $ grails create-app RestHelloWorld

**Update your `build.gradle` to include the Rest Client Builder dependencies which we will need later on**

    
    dependencies {
        // add the following line to the 'dependencies' section
        testCompile "org.grails:grails-datastore-rest-client:4.0.7.RELEASE"
    }

**Create an Integration Test**

    $ grails create-integration-test HelloWorld

**Create a test method inside the integration test**

Open up the created HelloWorldControllerSpec inside the `/src/integration-test/groovy/resthelloworld/` package

    package resthelloworld

    import grails.test.mixin.integration.Integration
    import grails.transaction.*
    import spock.lang.*
    import grails.plugins.rest.client.RestBuilder
    import grails.plugins.rest.client.RestResponse

    @Integration
    @Rollback
    class HelloWorldSpec extends Specification {
        
        def setup() { 
        }
        
        def cleanup() { 
        }
        
        def "Ask for a nice HelloWorld"() {
            given:
            RestBuilder rest = new RestBuilder()

            when:
            RestResponse response = rest.get("http://localhost:8080/helloworld/")

            then:
            response.status == 200

            and:
            response.json.message == "helloworld"
        }
    }

**Run your test**

    $ grails test-app

Offcourse this will fail as we do not have implement the controller yet.

**Create a Rest controller**
    
    $ cd RestHelloWorld
    $ grails create-controller HelloWorld

**Note:** The generation of the controller also create a Unit Test for the controller, default this test will fail. We are going to delete the generated Unit Test as we do not need it now. This test is located under the `/src/test/` groovy package.

        $ rm ./src/test/groovy/resthelloworld/HelloWorldControllerSpec.groovy

**Implement the controller function that will return data**

    package resthelloworld

    class HelloWorldController {
        
        def index() {
            render(status: 200, contentType: "application/json") {
                ["message" : "helloworld"]
            }
        }
    }
            
**Modify UrlMapping**
In order to get our newly generated controller accesible via Rest we need to modify our UrlMappings. 

    class UrlMappings {

        static mappings = {
            "/$controller/$action?/$id?(.$format)?"{
                constraints {
                    // apply constraints here
                }
            }

            "/"(view:"/index")
            "500"(view:'/error')
            "404"(view:'/notFound')

            // add the line below
            "/helloworld/"  (controller: "helloWorld", action: "index", method: "GET")
        }
    }

**Test your app**

    $ grails test-app

You should find that your tests are fine now :)

    $ grails test-app
    BUILD SUCCESSFUL

    Total time: 2.054 secs
    | Tests PASSED
