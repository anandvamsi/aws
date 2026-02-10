# How to monitor URL using cloudwatch sythatics:

A canary:
- Visits your website or API
- Acts like a real user
- Checks if things are working
- Tells you if something is broken
It runs automatically on a schedule (every 1 min, 5 min, etc.).

What can a canary check?

-  Website is reachable
-  API is responding
-  sponse time is OK
-  Response content is correct
-  Login flow works
-  Button click works (UI flow)

## Here is the procedure to create canary and check the heartbeat monitoring
- In the builder section provide name, and application ```endpoint url```
- In the script editor use the version sys-nodejs-puppeteer-13.1
Below script
```bash
const synthetics = require('Synthetics');
const log = require('SyntheticsLogger');

exports.handler = async () => {
    const requestOptions = {
        hostname: 'dplcheck-XXXXXXXXXXXXXXanXXtara.live',
        method: 'GET',
        path: '/api/test',
        protocol: 'http:',
        port: 80,
        headers: {
            'Cache-Control': 'no-cache'
        }
    };

    await synthetics.executeHttpStep(
        'HealthCheck',
        requestOptions,
        async (res) => {
            let responseBody = '';

            res.on('data', chunk => {
                responseBody += chunk;
            });

            res.on('end', () => {
                log.info(`Status Code: ${res.statusCode}`);
                log.info(`Response Body: ${responseBody}`);

          
                if (res.statusCode !== 200) {
                    throw new Error(`Unexpected status code: ${res.statusCode}`);
                }

                
                if (!responseBody.includes('DPLCheck 1.0 api called')) {
                    throw new Error(
                        `Unexpected response body: ${responseBody}`
                    );
                }
            });
        }
    );
};

```

- In Schedule section , use cron or  Run canary mention the minutes.
- In VPC settings, mention the vpc,subnets (make sure the subnets have NAT or internet gateway)and make sure security group have rule 0.0.0.0/0 for the port 53
- Choose ```create role`` at the end to complete the process.

