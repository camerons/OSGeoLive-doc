:Author: OSGeo-Live
:Author: Jachym Cepicky, Tom Kralidis, Luís de Sousa
:Reviewer: Cameron Shorter
:Version: osgeo-live10.0
:License: Creative Commons Attribution

.. Cameron Comment:
  Please address Review Comments (like this one), then remove the comments.
  I've made minor syntax changes without use of comments, so it is worth
  diffing with previous changes to see what I've changed and verify it is ok.

.. Cameron Comment:
  Please have a look at the Writing Tips in the uDig Quickstart (which is 
  our sample quickstart to follow).

.. Cameron Comment:
  The screen shots at scale 25% are unreadable. We recommend 50% scale for
  1024x768 screen shots, although if you keep screen shot small, you might
  find they are easier to read, (and still fit) if you keep at 70%.

.. image:: ../../images/project_logos/logo-pywps.png
  :scale: 100 %
  :alt: project logo
  :align: right
  :target: http://pywps.org

****************
PyWPS Quickstart
****************

PyWPS is an OGC WPS (Web Processing Service 1.0.0) server implementation written
in `Python <http://python.org>`_.

PyWPS is installed by default on OSGeo-Live.  This Quickstart describes how to:

* test a PyWPS installation
* configure the basics of a PyWPS instance
* create and deploy process in PyWPS
* perform `GetCapabilities`, `DescribeProcess` and `Execute` operations

.. contents:: Contents

Tester Application
==================

.. Cameron Comment: For all Quickstarts, we focus on "Keep it Simple".
  So just describe one way to start PyWPS, not 2. Ie, describe how to start
  PyWPS from the launcher, (which you can then go on to say opens up the web
  page at URL)

To run the PyWPS tester, use the PyWPS launcher from the Web Services group, or open Firefox and navigate to ``http://localhost/pywps/wps.py``:

.. Cameron Comment: Assume students are not very computer literate. So I suggest
  call GetCapabilities from the web page rather than using wget from command
  line. 
  
To test the `GetCapabilities` operation open you favourite internet browser and navigate to the following address::

    http://localhost/pywps/wps.py?service=WPS&version=1.0.0&request=GetCapabilities

The result will be a GetCapabilites response document, like the one in the example below::

.. code:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wps:Capabilities service="WPS" version="1.0.0" xml:lang="en-CA" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsGetCapabilities_response.xsd" updateSequence="1">
      <ows:ServiceIdentification>
         <ows:Title>PyWPS Server</ows:Title>
         <ows:Abstract>See http://pywps.wald.intevation.org and http://www.opengeospatial.org/standards/wps</ows:Abstract>
         <ows:Keywords>
            <ows:Keyword>GRASS</ows:Keyword>
            <ows:Keyword>GIS</ows:Keyword>
            <ows:Keyword>WPS</ows:Keyword>
         </ows:Keywords>
         <ows:ServiceType>WPS</ows:ServiceType>
         <ows:ServiceTypeVersion>1.0.0</ows:ServiceTypeVersion>
         <ows:Fees>None</ows:Fees>
         <ows:AccessConstraints>none</ows:AccessConstraints>
      </ows:ServiceIdentification>
      <ows:ServiceProvider>
         <ows:ProviderName>Your Company Name</ows:ProviderName>
         <ows:ProviderSite xlink:href="http://foo.bar"/>
         <ows:ServiceContact>
            <ows:IndividualName>Your Name</ows:IndividualName>
            <ows:PositionName>Your Position</ows:PositionName>
            <ows:ContactInfo>
               <ows:Address>
                  <ows:DeliveryPoint>Street</ows:DeliveryPoint>
                  <ows:City>City</ows:City>
                  <ows:PostalCode>000 00</ows:PostalCode>
                  <ows:Country>eu</ows:Country>
                  <ows:ElectronicMailAddress>login@server.org</ows:ElectronicMailAddress>
               </ows:Address>
               <ows:OnlineResource xlink:href="http://foo.bar"/>
               <ows:HoursOfService>0:00-24:00</ows:HoursOfService>
               <ows:ContactInstructions>none</ows:ContactInstructions>
            </ows:ContactInfo>
            <ows:Role>Your role</ows:Role>
         </ows:ServiceContact>
      </ows:ServiceProvider>
      <ows:OperationsMetadata>
         <ows:Operation name="GetCapabilities">
            <ows:DCP>
               <ows:HTTP>
                  <ows:Get xlink:href="http://localhost/cgi-bin/wps?"/>
                  <ows:Post xlink:href="http://localhost/cgi-bin/wps"/>
               </ows:HTTP>
            </ows:DCP>
         </ows:Operation>
         <ows:Operation name="DescribeProcess">
            <ows:DCP>
               <ows:HTTP>
                  <ows:Get xlink:href="http://localhost/cgi-bin/wps?"/>
                  <ows:Post xlink:href="http://localhost/cgi-bin/wps"/>
               </ows:HTTP>
            </ows:DCP>
         </ows:Operation>
         <ows:Operation name="Execute">
            <ows:DCP>
               <ows:HTTP>
                  <ows:Get xlink:href="http://localhost/cgi-bin/wps?"/>
                  <ows:Post xlink:href="http://localhost/cgi-bin/wps"/>
               </ows:HTTP>
            </ows:DCP>
         </ows:Operation>
      </ows:OperationsMetadata>
      <wps:ProcessOfferings>
         <wps:Process wps:processVersion="0.1">
            <ows:Identifier>dummyprocess</ows:Identifier>
            <ows:Title>Dummy Process</ows:Title>
            <ows:Abstract>The Dummy process is used for testing the WPS structure. The process will accept 2 input numbers and will return the XML result with an add one and subtract one operation</ows:Abstract>
         </wps:Process>
         <wps:Process wps:processVersion="2.0">
            <ows:Identifier>ultimatequestionprocess</ows:Identifier>
            <ows:Title>Answer to Life, the Universe and Everything</ows:Title>
            <ows:Abstract>Numerical solution that is the answer to Life, Universe and Everything. The process is an improvement to Deep Tought computer (therefore version 2.0) since it no longer takes 7.5 milion years, but only a few seconds to give a response, with an update of status every 10 seconds.</ows:Abstract>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>complexVector</ows:Identifier>
            <ows:Title>First Process</ows:Title>
            <ows:Abstract>Get vector imput and return it to output</ows:Abstract>
         </wps:Process>
         <wps:Process wps:processVersion="2.0">
            <ows:Identifier>complexRaster</ows:Identifier>
            <ows:Title>Second Process</ows:Title>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>noOutput</ows:Identifier>
            <ows:Title>No output given</ows:Title>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>firstInstance</ows:Identifier>
            <ows:Title>First instance process</ows:Title>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>secondInstance</ows:Identifier>
            <ows:Title>Second instance process</ows:Title>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>assyncprocess</ows:Identifier>
            <ows:Title>Assynchronous process</ows:Title>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>bboxprocess</ows:Identifier>
            <ows:Title>BBox process</ows:Title>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>complexprocess</ows:Identifier>
            <ows:Title>Complex process</ows:Title>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>literalprocess</ows:Identifier>
            <ows:Title>Literal process</ows:Title>
                           <ows:Metadata xlink:title="Foobar" xlink:href="http://foo/bar"/>
                           <ows:Metadata xlink:title="Barfoo" xlink:href="http://bar/foo"/>
                           <ows:Metadata xlink:title="Literal process" xlink:href="http://localhost/cgi-bin/wps?service=WPS&amp;request=DescribeProcess&amp;version=1.0.0&amp;identifier=literalprocess"/>
                           <ows:Metadata xlink:title="Literal process" xlink:href="http://foobar/"/>
         </wps:Process>
         <wps:Process wps:processVersion="None">
            <ows:Identifier>noinputsprocess</ows:Identifier>
            <ows:Title>No inputs</ows:Title>
         </wps:Process>
      </wps:ProcessOfferings>
      <wps:Languages>
         <wps:Default>
            <ows:Language>en-CA</ows:Language>
         </wps:Default>
         <wps:Supported>
            <ows:Language>en-CA</ows:Language>
         </wps:Supported>
      </wps:Languages>
      <wps:WSDL xlink:href="http://localhost/cgi-bin/wps?WSDL"/>
   </wps:Capabilities>

.. Cameron Comment: Context is required here. Assume minimal knowledge from the
  student. They don't know what a Get_Capabilities request is, or why you would
  call it, or what you will do with the result.
  And the result is coming back as XML, which is always daunting for
  computer illiterate people.
  
.. Luís: Describing what WPS is is out of the scope of this document.
  
.. Cameron Comment: Is it possible to access pywps from a GUI application? Ideally a browser
  based command console where you can construct queries and see results -
  similar to GeoServer, would be great. Alternatively, can you use QGIS or
  similar as a client?
  (This comment is applicable for rest of document too)
  
.. Luís: The only full featured WPS client that I am awere of is iGUESS: https://github.com/ERIN-LIST/iguess
  
This document describes the functionality provided by this WPS instance. After some metadata about this instance, the response documents lists all the processes available. The `DescribeProcess` operation can be used to know the details of each of these processes 

Lets select a process from the list, for example `ultimatequestionprocess`, and
display it's description by calling the `DescribeProcess` operation. Again in the browser, navigate to this address::

    http://localhost/pywps/wps.py?service=wps&version=1.0.0&request=DescribeProcess&identifier=ultimatequestionprocess

The server returns back a  `DescribeProcess` response document::

.. code:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wps:ProcessDescriptions xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsDescribeProcess_response.xsd" service="WPS" version="1.0.0" xml:lang="en-CA">
       <ProcessDescription wps:processVersion="2.0" storeSupported="false" statusSupported="false">
           <ows:Identifier>ultimatequestionprocess</ows:Identifier>
           <ows:Title>Answer to Life, the Universe and Everything</ows:Title>
           <ows:Abstract>Numerical solution that is the answer to Life, Universe and Everything. The process is an improvement to Deep Tought computer (therefore version 2.0) since it no longer takes 7.5 milion years, but only a few seconds to give a response, with an update of status every 10 seconds.</ows:Abstract>
           <ProcessOutputs>
               <Output>
                   <ows:Identifier>answer</ows:Identifier>
                   <ows:Title>The numerical answer to Life, Universe and Everything</ows:Title>
                   <LiteralOutput>
                       <ows:DataType ows:reference="http://www.w3.org/TR/xmlschema-2/#integer">integer</ows:DataType>
                   </LiteralOutput>
               </Output>
           </ProcessOutputs>
       </ProcessDescription>
   </wps:ProcessDescriptions>

Now let us Execute the `ultimatequestionprocess` process on the server. The process is,
according to it's description, able to *Answer to Life, the Universe and Everything*. No inputs are requied. Using the browser once more::

    http://localhost/pywps/wps.py?service=WPS&version=1.0.0&request=Execute&identifier=ultimatequestionprocess
    
As you might know, it takes a while, before the process is calculated. In our
case, it's about 10s. At the end, we can see the answer to life, universe and
everything::

.. code:: xml

   <?xml version="1.0" encoding="UTF-8"?>
   <wps:ExecuteResponse xmlns:wps="http://www.opengis.net/wps/1.0.0" xmlns:ows="http://www.opengis.net/ows/1.1" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.opengis.net/wps/1.0.0 http://schemas.opengis.net/wps/1.0.0/wpsExecute_response.xsd" service="WPS" version="1.0.0" xml:lang="en-CA" serviceInstance="http://localhost/cgi-bin/wps?service=WPS&amp;request=GetCapabilities&amp;version=1.0.0">
       <wps:Process wps:processVersion="2.0">
           <ows:Identifier>ultimatequestionprocess</ows:Identifier>
           <ows:Title>Answer to Life, the Universe and Everything</ows:Title>
           <ows:Abstract>Numerical solution that is the answer to Life, Universe and Everything. The process is an improvement to Deep Tought computer (therefore version 2.0) since it no longer takes 7.5 milion years, but only a few seconds to give a response, with an update of status every 10 seconds.</ows:Abstract>
       </wps:Process>
       <wps:Status creationTime="2016-07-12T17:13:52Z">
           <wps:ProcessSucceeded>PyWPS Process ultimatequestionprocess successfully calculated</wps:ProcessSucceeded>
       </wps:Status>
       <wps:ProcessOutputs>
           <wps:Output>
               <ows:Identifier>answer</ows:Identifier>
               <ows:Title>The numerical answer to Life, Universe and Everything</ows:Title>
               <wps:Data>
                   <wps:LiteralData dataType="integer">42</wps:LiteralData>
               </wps:Data>
           </wps:Output>
       </wps:ProcessOutputs>
   </wps:ExecuteResponse>
  
.. I think you are underselling the great value of a WPS here. I recommend
  that you select an geographic example which makes use of sample data
  already on OSGeo Live, and which returns a visual map that people can look
  at. Quickstarts are a marketing tool. Lets use it to sell pywps.

.. Luís: All the example processes returning complex outputs also require complex inputs. These are not practical in an example document like this. Also note that response documents are always XML, there is nothing visual about them. There are better examples comming with PyWPS 4.0.


Configuration
=============

You can configure a PyWPS instance in the `/etc/pywps/pywps.cfg`
configuration file. The values are self explaining, but you can always refer to
`standard documentation <http://geopython.github.io/pywps/doc/build/html/configuration/index.html#configuration-of-pywps-instance>`_

Processes
---------

You can find some example processes in the `/etc/pywps/processes`
directory. Every process is usually stored in separate file and is represented
by Python class, with constructor and `execute()` method::

.. code:: python
    
    # Example of PyWPS process (shorten)
    from pywps.Process import WPSProcess                               
    class Process(WPSProcess):
     def __init__(self):
         WPSProcess.__init__(self,
                             identifier="ultimatequestionprocess", #the same as the file name
                             ....
                                           
     def execute(self):
         import time
         self.status.set("Preparing....", 0)
         for i in xrange(1, 11):
             time.sleep(2)
             self.status.set("Thinking.....", i*10) 
         #The final answer    
         self.Answer.setValue("42")

.. image:: ../../images/screenshots/800x600/pywps-editor.png
  :scale: 50%
  :alt: screenshot of the editor
  :align: center

  Same address in the browser

Directory of your process deployment is configured within the
`PYWPS_PROCESSES` environment variable.

.. Cameron Comment:
  I assume the pyWPS plugin will have a good GUI. I suggest you should
  create one example which makes use of this plugin. 
  
.. Luís: PyWPS is not a plug-in, nor a GUI. Please take a look at the PyWPS presentation: https://github.com/PyWPS/presentation 

What Next?
==========

For more information on PyWPS, please consult the `documentation`_ on the PyWPS website.

Things to Try
=============

If you like PyWPS, you may also try the upcoming `PyWPS 4.0`_ release.

You can also try to install the `WPS Plugin <https://plugins.qgis.org/plugins/wps/>`_ for the
`QGIS project <en/quickstart/qgis_quickstart.rst>`_. While usefull, this plugin fails to display properly a number of complex outputs.

.. image:: ../../images/screenshots/1024x768/pywps-qgis.png
  :scale: 50%
  :alt: pywps qgis
  :align: center


.. _`OpenGIS Web Processing Service`: http://www.opengeospatial.org/standards/wps
.. _`Open Source`: http://www.opensource.org/
.. _`documentation`: http://pywps.org/docs
.. _`lxml`: http://lxml.de/
.. _`Download`: http://pywps.org/download
.. _`GitHub`: https://github.com/geopython/PyWPS
.. _`PyWPS 4.0`: http://pywps.readthedocs.io