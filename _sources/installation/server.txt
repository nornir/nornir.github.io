Data Server Setup
=================

The volumes Nornir builds often are too large to store on a workstation.  Even if a users local workstation is used for storage some reporting features and compatibility with web viewers require the data to be accessible via HTTP.  A big advantage of Nornir is easily accessing data over the network.  At the Moran Eye Center all TEM captures are sent out as URL links in E-mail.  This frees our TEM from use as browser.

HTTP Server Config
------------------

Place all volumes under a single directory.  Make that directory the root of a web server such as IIS or Apache.  

Create an about.xml file in the root of your web server.  This the example from the Marc lab 

::

  <Volume host="http://internal.connectomes.utah.edu/">

host should be set to the URL used to access the directory about.xml resides with.  Appending about.xml to your URL should pull up your about.xml file in your browser, for example ``http://internal.connectomes.utah.edu/about.xml``

Mime Types
----------

Nornir servers should map the following MIME types to plain/text

==========  ===============
extension   MIME Type
==========  ===============
.mosaic     plain/text
.stos       plain/text
.vikingxml  application/xml
==========  ===============

Annotation Databases
--------------------

Most users can ignore this section, but additional about.xml files can be added in subdirectories.  Elements and attributes are located and used as needed.  The about.xml file below injects instructions on which annotation database to use for a volume. 

::

  <Volume>
    <VolumeToEndpoint Authentication="https://connectomes.utah.edu/Services/Authentication/Authentication.svc" Endpoint="https://connectomes.utah.edu/Services/RC2Binary/Annotate.svc"/>

    <DefaultWebAnnotationUserSettings Uri="http://internal.connectomes.utah.edu/RC2/WebAnnotationUserSettings.xml"/>
  </Volume>
