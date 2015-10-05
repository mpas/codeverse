+++
title = "Run a Grails 3 generated Fat Jar file in production mode"
date = "2015-06-11"
categories = ["Development"]
tags = ["grails"]
+++

When creating a Grails WAR/JAR file using:

    $ grails war

The resulting artifact can be run in production mode using:

    $ java -Dgrails.env=prod -Dserver.port=9000 -jar <name-of-jar-file>.jar