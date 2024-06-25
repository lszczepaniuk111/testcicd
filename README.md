
# UI(Webv3) test case automation
## Running tests in docker

### **Prerequisite**
 * [docker](https://docs.docker.com/get-docker/)
 * [docker-compose](https://docs.docker.com/compose/)


1. Build docker image

```bash
docker-compose build
```
2. Prepare .env file

The `.env` file holds all the **default test parameters and settings** and has been separated from the docker-native files to be more transparent and easy to modify.

`.env` is not in repository and needs to be created. To do this follow this steps:
1. Duplicate template env file e.g `example_stage.env`.
2. Rename duplicate to `.env`.
3. Set constants with proper data.

### Running specific functional test suite

    docker-compose up ts_authentication
### Running specific layout test suite

    docker-compose up ts_authentication_l

When tests are done, output will be generated in `app\logfiles`(Playwright) and `app\report` (Galen)

## Develop functional test cases
###  **Prerequisite**

RF Browser(Playwright)  library installation requires both Python™ and Node.js® for more details please follow https://robotframework-browser.org/

### Managing environment variables for local runs on Windows

For manual local runs on Windows outside the Docker, you might have to set environment variables to be available in the next terminal session. \
You can manage environment variables by running `.bat` scripts from the `./scripts` folder in CMD as Administrator. \
Scripts will use the variables from your `.env` file.

###  **Test case creation** 
For start creating test cases using Browser library please look at https://robotframework-browser.org/#examples \
Browser advanced keywords documentation can be found in https://marketsquare.github.io/robotframework-browser/Browser.html \

Please also follow this guideline [Guideline](https://eagleeyenetworks.atlassian.net/wiki/spaces/ENG/pages/1739424016/RobotFramework+guidelines) 
during test case creation!
###  **Locally Run**


1- Run single test -> ```python -m robot app/tests/ts_authentication/SignInPage.robot```

2- Run by folder -> ```python -m robot app/tests/ts_authentication/```)

3- Run test and get logfiles -> ```python -m robot -r report -d ./logfiles app/tests/ts_authentication/ ```

> **Notice:** for debug(no headles mode) please change New Page keyword to Open Browser  

## Develop layout test cases
### **Prerequisite**

For Galen you need to have a Java version 1.8 or greater installed.  \
Also webdriver e.g geckodriver is need to be installed  \
For more instalation details please follow http://galenframework.com/docs/getting-started-install-galen/

### **Test case creation** 
For start creating test cases using Galen Framework please look at http://galenframework.com/docs/tutorial-first-project/ \
More advanced documentation can be found in \
http://galenframework.com/docs/reference-galen-spec-language-guide/ \
and \
http://galenframework.com/docs/reference-galen-test-suite-syntax/

Feel free to read all Galen Framework [Documentation](http://galenframework.com/docs/all/)!
### **Locally Run**

1- Run single test -> ```galen test tests/ts_authentication/login_page.test --htmlreport report --config "galen.config"```

2- Run by folder -> ```galen test tests/ts_authentication --htmlreport report --config "galen.config"```)

3- Run test and get report -> ```galen test tests/ts_authentication/login_page.test --htmlreport report --config "galen.config" ```


> **Notice:** for debug(no headles mode) please edit  galen.browser.headless in 'app/galen.config' file 

## Troubleshooting

### Dockerfile - docker build fails at step  `RUN apt install -y npm`

If you see errors like below, then it is most likely a network load balancer issue of [deb.debian.org](deb.debian.org).

``` bash
E: Failed to fetch http://deb.debian.org/debian/pool/main/n/node-strip-json-comments/node-strip-json-comments_4.0.0-4_all.deb  403  connecting to deb.debian.org:80:
(...)
A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond.
(...)
failed to solve: process "/bin/sh -c apt install -y npm" did not complete successfully: exit code: 100
```

Use these alternative sources before downloading packages (you can try other [mirrors](https://www.debian.org/mirror/list.en.html) as well):

``` docker
    RUN echo "deb http://ftp.us.debian.org/debian/ stable main contrib non-free" > /etc/apt/sources.list \
    && echo "deb-src http://ftp.us.debian.org/debian/ stable main contrib non-free" >> /etc/apt/sources.list
```
