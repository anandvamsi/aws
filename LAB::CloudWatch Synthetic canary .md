# How to monitor URL (REST,GET) API URLs inside AWS

We can use CW synthetics canary for this which is cheaper than other vendor tool at the same we can export logs,alerts all under one roof
consider the URL ```http://dplcheck-eawwi-test.apps.abc.live/api/test``` which is GET request

- step 1: Open the cloudwatch systhetics canaries and

- step 2: Fill in the following details
            - name
            - Runtime environement: ```syn-nodejs-puppeter-13.1
            - paste the below code
              ```bash


          const synthetics = require('Synthetics');
          const log = require('SyntheticsLogger');

          const apiCanary = async () => {
          
            const apiEndpoints = [
            'http://<Mentioned the URL>/api/test'
            ];
          
            for (const url of apiEndpoints) {
          
              const stepName = `GET_api_check_${url.split('/').pop()}`;
          
              await synthetics.executeHttpStep(
                stepName,
                {
                  method: 'GET',
                  url: url,
                  headers: {
                    'Accept': 'application/json'
                  },
                  timeout: 5000,
                  followRedirects: true
                },
                (response) => {
                  log.info(`URL: ${url}`);
                  log.info(`Status: ${response.statusCode}`);
          
                  if (response.statusCode !== 200) {
                    throw new Error(`Failed with status ${response.statusCode}`);
                  }
                }
              );
            }
          };
            exports.handler = async () => {
            return await apiCanary();
        };
  ```

- step 2a : Run canary, select the schedule in which canary has to execute
           - it start with 1 minute,daily or cron expression will do.
  
- step 3: Retain  rest of setting except vpc settings, select the respective VPC in which URL can be accessed and private subnet and security
  group with port open 80/443 open to respective CIDR(VPC CIDR) and save it.

  
