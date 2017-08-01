# Cuckoo integration with DXL

## Overview
The OpenDXL Cuckoo integration allows the Cuckoo analysis engine to query McAfee TIE over DXL for 
file reputation values. That information is included in the Cuckoo results array. Additionally, 
Cuckoo reporting is updated to include results in the HTML report.

The Processing module works on Cuckoo 1 and 2. The reporting tieupdate module only works on Cuckoo 2.
Follow version specific instruction for updating the processing and reporting modules.

## Prerequisites
* The integration requires a successful installation of Cuckoo. 
* Install the required dependencies with the requirements.txt file on the system that is running Cuckoo.
    ```sh
    $ pip install -r requirements.txt
    ```
    This will install the dxlclient and the dxltieclient modules.
    
## Installation
* Download the [Latest Release](https://github.com/opendxl-community/cuckoo/releases)
    Extract the release .zip file.
* Follow the steps for [Certificate creation](https://opendxl.github.io/opendxl-client-python/pydoc/certcreation.html) 
and [Configuration](https://opendxl.github.io/opendxl-client-python/pydoc/sampleconfig.html)
* In the tie.py file modify the location of the dxlclient.config file by updating the CONFIG_FILE variable to point to the config file.
* Make a backup of <install dir>\common\config.py. Overwrite the file with the config.py from the release zip file.
* Copy the updated tie.py to Cuckoo.Refer to the [Cuckoo docs](http://docs.cuckoosandbox.org/en/latest/installation/host/configuration/#processing-conf) for adding
processing modules and the [wiki](https://github.com/opendxl-community/cuckoo/wiki/tie.py) for instructions.
    * Refer to [Processing Modules](http://docs.cuckoosandbox.org/en/latest/customization/processing) for more details.
    * Make sure that the processing.conf file is updated to enable the tie processing module
         ```
         [tie]
         enabled = yes
         ```
* [Follow the instructions](https://github.com/opendxl-community/cuckoo/wiki/report.html) to copy the html files.
    * Refer to the [wiki](https://github.com/opendxl-community/cuckoo/wiki/base-report.html) for the base report.html.
    Note: The instructions in the wiki are specific to version.Follow version specific instruction for updating the files.    
* If you are using Cuckoo V2.0+ you can also use the tieupdate.py file to set reputations on TIE.
  * Change the config file location similar to tie.py in the tieupdate.py and follow the [Cuckoo docs](http://docs.cuckoosandbox.org/en/latest/installation/host/configuration/#reporting-conf)
    instructions for enabling reporting modules.
    * Also refer to [Reporting Modules](http://docs.cuckoosandbox.org/en/latest/customization/reporting/) for more details.
  * The tieupdate.py file makes a SetReputation call to the TIEServer. Ensure that you have updated the 
  [Authorization rules](https://opendxl.github.io/opendxl-tie-client-python/pydoc/basicsetreputationexample.html)
  in ePO to allow this client to set reputations.
      ```  
    [tieupdate]
    enabled = on
    ```
  * In Cuckoo ensure that you have enabled creation of report.html. Refer to Cuckoo documentation for version specific instructions for enabling generation of report.html.
    * In Cuckoo V2.x, in the singlefile section enable reporting as shown below. You can choose to turn on pdf as needed.   
      ```
        [singlefile]
        # Enable creation of report.html and/or report.pdf?
        enabled = yes
        # Enable creation of report.html?
        html = yes
        # Enable creation of report.pdf?
        pdf = no
      ``` 
* Restart Cuckoo.

## Usage
* Start Cuckoo.
* Start the [Cuckoo Web interface](http://docs.cuckoosandbox.org/en/latest/usage/web/).    
* Start [Cuckoo API Server](http://docs.cuckoosandbox.org/en/latest/usage/api/) to view the TIE reports via REST API.
* Submit samples to Cuckoo via CLI or the UI and view the [TIE Reputation](https://github.com/opendxl-community/cuckoo/wiki) reports.
    * After the task has completed , use [Rest API](http://docs.cuckoosandbox.org/en/latest/usage/api/#resources) to view the reports.
        * Sample task report <server>:8090/tasks/report/<ID of the task>/html is shown in the [wiki](https://github.com/opendxl-community/cuckoo/wiki).
        





