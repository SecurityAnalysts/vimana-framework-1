# Vimana Framework
Vimana is a modular security framework designed to audit Python applications.
![Alt text](imgs/s0v6.png?raw=true "VIMANAFRAMEWORK")

## Content
1. [ Framework Structure. ](#struct)
2. [ Starting with Vimana. ](#starting)
3. [ About this Version. ](#about)
4. [ Curiosities. ](#curio)
5. [ Vimana is not](#vin)
6. [ Acknowledgment. ](#ack)
7. [ Wiki. ](#wiki)
8. [ Disclaimer. ](#disc)


<a name="struct"></a>
### Framework Structure

The base of the Vimana is composed of crawlers focused on frameworks (in addition to the generic ones for web), trackers, discovery, fuzzer, parser among other types of modules. The main idea, from where the framework emerged, is to identify, through a blackbox approach, configuration flaws and inadequate and/or insufficient implementations that allow unhandled exceptions to be triggered. Depending on the framework settings (or specific libs even when not using frameworks, for example raw wsgi) this can lead to leakage of sensitive and critical information that can allow to compromising the entire application, server, apis, databases, services and any third part software with tokens, secrets or api keys in current exposed environment variables.

Another important step performed by Vimana is to obtain and reconstruct the source code snippets of the affected modules (leaked by exceptions) that allow the discovery of hardcoded credentials, connection strings to databases, vulnerable libraries, in addition to allowing the analysis of logic of the application of a mixed perspective between the black and whitebox approaches, since the initial analysis starts from a blind test, but ends up allowing access to code snippets.


<a name="starting"></a>
### Starting with Vimana

The easiest way is through Docker image build script:
```
sudo git clone https://github.com/s4dhul4bs/vimana-framework.git
cd vimana-framework && sudo ./vmnf_build
```
If the build runs smoothly, you will see the about screen (README image above)

If you prefer you can follow the same steps as the script manually to build the image:
```
timedatectl set-ntp yes
sudo git clone https://github.com/s4dhul4bs/vimana-framework.git
cd vimana-framework
sudo docker build --no-cache=true --network=host -t vimana_framework:alpha .
```
Once the image has been successfully created, you can start Vimana as follows:

```sudo docker run -it --name vimana vimana_framework:alpha about```

And the same image will be displayed.

Of course, the framework can also be executed in the traditional way. directly by code (most stable way so far):
```
sudo git clone https://github.com/s4dhul4bs/vimana-framework.git
cd vimana-framework
python3 vimana.py 
```

Example of running a siddhi (vimana module): 

```sudo docker run -it vimana_framework:alpha run --module dmt --target-list 192.168.1.101,192.168.1.212,mypythonapp.com --port-list 5000,5001,8000 --verbose --debug --random --threads 5```


Explaining the command line syntax above:

```run```         Command to run a siddhi (vimana framework module) in inline mode (in this case, module DMT, Django Misconfiguration Tracker)

```--target-list``` The framework supports several types of scope definition arguments (although this also depends on the arguments expected by each module). In this case, a list of IPs and URLs was used with the argument target-list (comma-separated).

```--port-list``` Here, as with the definition of targets, the ports also accept various formats, in this case the port-list with a list of ports to be tested is being used. An important note, when you do not want the defined port to be tested before the chosen module is invoked, the `` --ignore-state`` argument must be passed so that the state of the port will not be checked.

```--verbose```   Enable verbose mode in realtime issues presentation

```--debug```     Enable Vimana debug mode,showing realtime technical information about execution 

```--random```    Enable randomize, this flag, enable randomization in supported modules (for example user-agent, cookies, tokens, etc) 

```--threads```   Configures the number of threads to be executed by the modules involved in the analysis (those that support threads). 

To know the arguments supported by a siddhi, use the syntax: ```vimana.py args --module <module_name>```

<a name="about"></a>
### About this Version

Most of the features have been exhaustively tested for a long time against different scenarios and observed carefully, however, the tool has acquired a considerable size and complexity and therefore, there will certainly be some bugs not known at the moment. The known ones will be documented in issues. Feel free to contact us if you have suggestions, collaborations, or anything else related.

Another point about this alpha version is that the main features (siddhis) are focused on Django framework with application running in the vast majority in homologation and/or production scenarios with DEBUG true. 

**For the next releases**

In addition to the general improvements in the entire framework structure and in the siddhis already available, there are other important points that are:

* Features for tracking and fingerprinting template engines (like Genshi, Jinja, Mako, etc.)  
* Resources for automated SSTI tests (Server Side Template Injection)
* New fuzzers rules for Django 
* Some lib exploits
* Resources to generate custom payloads on app context
* New siddhis focused on other development frameworks such as Flask, Web2py, Bottle and so on)
* App Crawler
* DB integration
* Logging
* Siddhis to test service according with leak contexts (abduct)


<a name="curio"></a>
### Curiosities

**The genesis of the tool**

The idea for the tool came up in 2010 during an intrusion test where (in those typical scenarios without many relevant vulnerabilities) I found numerous IPs on the internal network, with some python applications also exposed to the internet running with Django framework, WSGI and Flask, some with DEBUG true, well, it was a scenario with a lot of relevant information that could enable me to compromise the company's internal communication channels, services and developer accounts. However, I was unable, within the stipulated period, to analyze, collect and test everything that was leaked, so (in the height of despair) I imagined a ship abducting all that and putting together a report to save the project, but I didn't have one dedicated to such a purpose, so I decided to start a new one in the way I imagined.

**About the name**

According to Sanskrit texts the ancients had several types of airships called vimanas. These vehicles were used to fly through the air from city to city; to conduct aerial surveys of uncharted lands; and as delivery vehicles for awesome weapons.

<a name="vin"></a>
### Vimana is not

Vimana is not a vulnerability scanner, at least non-traditional, because it does not look directly for flaws like SQLi, XSS, XXE, RFI and so on. Instead, the main focus of the framework, which is also its main feature, is to perform fuzzing to trigger exceptions and from there feed other modules that can perform other tasks from that initial input. However, when I spoke about the research that led to this tool, I showed that in some cases it was possible to identify traditional vulnerabilities (such as those in the OWASP Top 10) by analyzing an exception that was triggered. So, there are already plans for a siddhi to identify these vulnerabilities, however, it is important to keep in mind that vimana is not intended to be a tool to exploit a possible sqli injection, for this it has sqlmap and several others.

There are certain parts where scope definitions are made (against which a particular module will be executed), where basic tests are made to check the state of the port informed as an argument on the command line. However, it is also important to note that this tool is not intended to be a port and service scanner, for this there is nmap and others.

And so on, what this framework is I don't know yet, it's early, but I can already point out some things that it is not and does not intend to be.

<a name="ack"></a>
### Acknowledgment

Special thanks to the guys from [AlligatorCon](https://alligatorcon.com) and [NullByte](https://nullbyte-con.org) conferences who gave me the opportunity to show a little bit about the research that resulted in the tool.


<a name="wiki"></a>
### Wiki
Soon we will have a wiki with exploration scenarios using the framework.

<a name="disc"></a>
### Disclaimer
The project aims to materialize an idea resulting from real audit scenarios for which there was no alternative to take advantage of the vectors in question. Therefore, it has a proposal to stimulate research in this context. The author of the project is not responsible for misuse, damage or loss caused by the use of the project, nor does it offer any warranty.
