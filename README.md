[![Build Status](https://travis-ci.org/Nikolas-Charalambidis/4IT475.svg?branch=master)](https://travis-ci.org/Nikolas-Charalambidis/4IT475)
[![codecov](https://codecov.io/gh/Nikolas-Charalambidis/4IT475/branch/master/graph/badge.svg)](https://codecov.io/gh/Nikolas-Charalambidis/4IT475)
[![codebeat badge](https://codebeat.co/badges/d29cc28c-cf3a-426d-89b4-21560d4c4a1d)](https://codebeat.co/projects/github-com-nikolas-charalambidis-4it475-master)
[![GitHub](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/Nikolas-Charalambidis/4IT475/blob/master/LICENSE)

# Cypher SOAP

## ⚠ No longer maintained nor published

A Soap Web Service as a semestral work for the 4IT475 System Integration Testing course.

**Disclaimer:** The web service Azure hosting was based on the univeristy account and is probably shut down. 

The goal of the semestal work is to design and implement a SOAP WS. This project represents a web service encrypting a plaintext using a cipher operation and decrypting it back. Some of the ciphers use keys or an additional configuration within the operation. The entire SOAP WS shall be published on Microsoft Azure.

We expect to deliver the solution in time. We have set the due date before the presentation of the results which is on 28th of November.

## [https://4it475.azurewebsites.net/soap/ws/cipher.wsdl](https://4it475.azurewebsites.net/soap/ws/cipher.wsdl)
Feel free to browse a list of all the [operations](https://github.com/Nikolas-Charalambidis/4IT475/wiki/Operations) published at our Wiki.

## Contributors
* Nikolas Charalambidis as [Nikolas-Charalambidis](https://github.com/Nikolas-Charalambidis)
* Alexandra Kolpakova as [alexKolpakova](https://github.com/alexKolpakova) & [kubik335](https://github.com/kubik335)

## Solution design
We chose Java as an implementing language, because it's the major language taught on the [university](https://fis.vse.cz/english/) we study. We decided to pick the following frameworks and libraries because they are easily configurable, broadly used among a large number of enterprises and open-source (except Azure):

* [Spring](https://spring.io/) enhanced with [Spring-Boot](http://spring.io/projects/spring-boot) to glue the web service with.
* [jUnit 4](https://junit.org/junit4/) to be sure that our smallest components are reliable and working.
* [Apache Maven](https://maven.apache.org/) for project build and dependency management.
    * [Spring Boot Maven Plugin](https://docs.spring.io/spring-boot/docs/current/maven-plugin/usage.html) to help us run the webservice "in-place".
    * [Maven WAR Plugin](https://maven.apache.org/plugins/maven-war-plugin/) to pack the web service into deployable *war* file.
    * [Codehaus Mojo JAXB2 Maven Plugin](http://www.mojohaus.org/jaxb2-maven-plugin/Documentation/v2.2/) to generate XSD schemas and serialize XML messages with using the [JAXB XJC binding compiler](http://www.mojohaus.org/jaxb2-maven-plugin/Documentation/v2.3.1/xjc-mojo.html).
    * [WSDL4J](https://mvnrepository.com/artifact/wsdl4j/wsdl4j) to generate the WSDL.
* [Microsoft Azure](https://azure.microsoft.com/en-us/) as a platform for deployment, which provides a free pricing tier for the students of our university.
* [Rattle](https://github.com/Nikolas-Charalambidis/Rattle) to keep our Azure webapp [alive](https://rattle.azurewebsites.net/). In case Rattle is down, you need to wait up to 1-2 minutes to boot the web service up.

### Problems
* [Issue #3](https://github.com/Nikolas-Charalambidis/4IT475/issues/3) We failed to break all the ciphers in the `cipher.xml` file into smaller pieces. The problem is that JAXB2 generates namespace for all the ciphers separately.
* [Issue #4](https://github.com/Nikolas-Charalambidis/4IT475/issues/4) We failed to use `XsdSchemaCollection` using `DefaultWsdl11Definition` to break the large schema `cipher.xml`. The NPE is thrown somehow and the Spring issue [SWS-1041](https://jira.spring.io/browse/SWS-1041) has been created.

### Nice to have
* [Issue #5](https://github.com/Nikolas-Charalambidis/4IT475/issues/5) We didn't configure the WAR file autodeployment to the Azure  using TravisCI upon every commit (merge) to the production master branch.

## Conclusion
* Although the project development is finished and no more functionality will be added, we welcome any pull-request to improve our project.
* We succeed to create our first SOAP web service and deliver it in time.
* We enlargened our skill-set.
* We published the project as an open-source and offer it to other students as a study material.
* Enjoy the WSDL published on [https://4it475.azurewebsites.net/soap/ws/cipher.wsdl](https://4it475.azurewebsites.net/soap/ws/cipher.wsdl) and check all the [operations](https://github.com/Nikolas-Charalambidis/4IT475/wiki/Operations).

# Sample usage

Let's learn how to use the Caesar cipher encryption and decryption.

1. Open SoapUI or any similar SOAP client and create a new SOAP WS entering the WSDL located on [https://4it475.azurewebsites.net/soap/ws/cipher.wsdl](https://4it475.azurewebsites.net/soap/ws/cipher.wsdl). 
2. Open the **CaesarEncrypt** operation at the `http://localhost:8080/soap/ws` endpoint.
3. Fill in the plaintext to be ecrypted inside the `<fis:encrypt>` tag. Optionally fill in `<fis:shift>` which stands for the shift in the international alphabet which is used by the Caesar cipher. The `<fis:shift>` is optional and the default value is `3`.
4. The full request should look like:
 
        <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" 
                          xmlns:fis="https://fis.vse.cz/">
            <soapenv:Header/>
            <soapenv:Body>
                <fis:CaesarEncryptRequest>
                    <fis:encrypt>Hello world</fis:encrypt>
                    <fis:shift>5</fis:shift>
                </fis:CaesarEncryptRequest>
            </soapenv:Body>
        </soapenv:Envelope>
       
5. Submit the request to the endpoint.
6. The response should look like:
 
        <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
            <SOAP-ENV:Header/>
            <SOAP-ENV:Body>
                <ns2:CaesarEncryptResponse xmlns:ns2="https://fis.vse.cz/">
                    <ns2:encrypted>MJQQT BTWQI</ns2:encrypted>
                </ns2:CaesarEncryptResponse>
            </SOAP-ENV:Body>
        </SOAP-ENV:Envelope>

7. The encrypted value is `MJQQT BTWQI`.
8. Use the **CaesarDecrypt** operation at the very same endpoint to receive the original value back.
 9. The full request should look like:
 
        <soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" 
                          xmlns:fis="https://fis.vse.cz/">
            <soapenv:Header/>
            <soapenv:Body>
                <fis:CaesarDecryptRequest>
                    <fis:decrypt>MJQQT BTWQI</fis:decrypt>
                    <fis:shift>5</fis:shift>
                </fis:CaesarDecryptRequest>
            </soapenv:Body>
        </soapenv:Envelope>
10. Upon submitting the request to the endpoint, the response should return back `HELLO WORLD` in the response:
 
         <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
             <SOAP-ENV:Header/>
             <SOAP-ENV:Body>
                 <ns2:CaesarDecryptResponse xmlns:ns2="https://fis.vse.cz/">
                     <ns2:decrypted>HELLO WORLD</ns2:decrypted>
                </ns2:CaesarDecryptResponse>
             </SOAP-ENV:Body>
         </SOAP-ENV:Envelope>
         
11. Please, do not spam requests. :)

The full list of all the operations of the endpoint to be found at [Wiki](https://github.com/Nikolas-Charalambidis/4IT475/wiki).

## Localhost installation

Build and run the project using command:

`mvn clean install`

In case the generating of target classes from the schema files is required, run:

`mvn jaxb2:xjc`

The WSDL should be available on the address:

[http://localhost:8080/soap/ws/cipher.wsdl](http://localhost:8080/soap/ws/cipher.wsdl)
