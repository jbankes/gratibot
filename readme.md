# Archived

This project has been archived in favor of our new version that can be found at [GitHub: Liatrio/Gratibot](https://www.github.com/liatrio/gratibot)

# Gratibot

Gratibot is based off of [HowdyAI's Botkit](https://botkit.ai/) framework. We appreciate all the hard work they have done to give us this framework to extend.

## Local Development

If you don't already have a Slack app for testing/development create a new one
1. Goto [api.slack.com/apps](https://api.slack.com/apps).
2. Add an App Name like YOUR_NAME-bot.
3. Select a Development Slack Workspace you want to test the bot in.
4. Press the `Create App` button.

Run bot locally and expose port
1. Clone project `git clone https://github.com/liatrio/gratibot.git`
2. Start ngrok `ngrok http 3000`
3. Edit .env file and add the `clientId`, `clientSecret` from the Slack app you create and set `PORT` to 3000. Also add `botHostname` and set it equal to the hostname generated by ngrok, https://NGROK_HOST
4. Start docker containers `docker-compose up --build`

Configure the follow Slack app features settings under each menu item
1. Bot User: Set display name and default username and Save Changes
2. OAuth & permission: Add a redirect URL which points to the ngrok forward `https://NGROK_HOST/oauth`
3. Event Subscriptions: Enable events, Set Request URL to ngrok forward `https://NGROK_HOST/slack/receive` and subscribe to bot events for `message.channels`, `message.groups`, `message.im`, `message.mpim`.

Connect bot to Slack
1. Goto [https://NGROK_HOST/](#) and follow instructions to add bot to Slack.


## AWS Manual/Admin Requirements 
The infrastructure code cannot be set up without the appropriate security pieces in place include:
* KMS key for the parameters
* Parameter store set up for slack credentials (clientId, clientSecret, clientSigningSecret)
* IAM Role to allow ECS task to utilize the parameters and key

This can be done manually or via the `infra_admin` Terraform configuration. This must be run by someone with the ability to create roles, keys, and parameters. 

For more information on the AWS Requirements check out [The ECS Docs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/specifying-sensitive-data.html#secrets-create-taskdefinition)

## Jenkins configuration
Jenkins needs the [AWS Credentials Plugin](https://wiki.jenkins.io/display/JENKINS/CloudBees+AWS+Credentials+Plugin) installed and configured with `AWS-SVC-Jenkins-non-prod-dev` and `AWS-SVC-Jenkins-prod-dev`
