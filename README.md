# alexa-aws-administration / VoiceOps
Do various administration tasks in your AWS account using your Amazon Echo.

This project started as a demo at the [Utah AWS Meetup](http://www.meetup.com/utah-aws/) in February 2016.

See the blog post on the 1Strategy Blog: [http://www.1strategy.com/blog/utah-aws-lambda-alexa/]( http://www.1strategy.com/blog/utah-aws-lambda-alexa/)

See a video of [this project in action on YouTube](https://www.youtube.com/watch?v=YH1P8ckFoLU).

## Lambda Function
The Lambda function is organized into "intent" handlers (with associated helper methods) with some boilerplate code for selecting the correct handler to run based on what the Alexa service determines the user is trying to do.

The onIntent function maps the intent that Alexa believes the user is trying to invoke, and executes the corresponding function to handle that intent. Here's a brief summary of the intents that are currently supported:
<h3>Welcome Response (getWelcomeResponse)</h3>
This function provides an introduction when a user launches the service on their Echo. It provides instructions on available commands and suggests an initial command to use, in this case "Select a region to use by saying, set the region to Virginia."
<h3>Set Region (setRegion)</h3>
When this intent is triggered, the user will have specified a desired region to work in and that region will be set in session (to be used in later requests).
<h3>Get Region (getRegionFromSession)</h3>
Will echo back which region is currently in session.
<h3>Get Instance Count (getInstanceCount)</h3>
Returns the number of running instances in the region that is currently in session.
<h3>Terminate Untagged Instances (terminateUntaggedInstances)</h3>
Will filter your instances and find those that are untagged (including instances with a blank name tag), and then terminate those instances. For more information on the code that does the filtering and terminating, please see <a href="http://www.1strategy.com/blog/use-aws-lambda-terminate-untagged-ec2-instances/" target="_blank">http://www.1strategy.com/blog/use-aws-lambda-terminate-untagged-ec2-instances/</a>.
<h2>Preparing the Lambda Function to Be an Alexa Skill</h2>
Make sure to create the Lambda function in the "N. Virginia (us-east-1)" region, as that it is the only region that supports receiving events from Alexa Skills Kit. After saving your Lambda function, make note of the Lambda's ARN, we'll need that when we setup the Alexa Skill.

In addition, we'll need the following items:
<h3>intents.json</h3>
The Alexa Skills Kit needs to know what intents are available in your new skill. This is done by providing a JSON document that describes each of the intents. You may notice that the SetRegionIntent has an additional "slots" property. You can think of a "slot" as a variable that the user provides. In this case, we want the user to specify a region variable, so we know which AWS region to operate against. You'll also notice that it references a "LIST_OF_REGIONS" type. We're going to define that type now.
<h3>LIST_OF_REGIONS</h3>
We want to limit the number of choices that can be used for the region slot (variable). We don't want the user to be able to specify a location that doesn't exist, so we limit what is accepted by supplying a list of valid words for this slot. Currently, we're limiting the options for regions to "Virginia" and "Oregon". To support more regions, we would simply add additional lines to this file and then ensure that the getRegionIdentifier function in our Lambda knows how to map the additional choices.
<h3>Sample Utterances</h3>
The Alexa Skills Kit uses machine learning to determine what the user is intending to do. We feed example phrases into the Alexa Skills Kit as input, including the desired intent to be triggered. We supply these examples by defining "sample utterances." We're supplying just a few examples of things users could say, and what intent should triggered. Note the use of the "{Region}" placeholder in some of the SetRegionIntent phrases. This is a link to the slot discussed earlier.
<h2>Testing your Alexa Skill</h2>
Now that we have our Lambda and the resources that the Alexa Skills Kit needs, let's put it all together and test it out. Start by heading to http://developer.amazon.com. If you haven't registered for an Amazon Developer Account, do so now, it's free! In order to test on your own Echo, make sure the email address you're using for a developer account matches that email address to which your Echo is registered.

Once logged in to the Amazon Developer Console, navigate to "Apps and Services" -&gt; "Alexa". Click the "Add a New Skill" button.
<h3>Skill Information</h3>
Fill in the required fields. The "invocation name" will be the identifier you use to open up this skill. For example, I used "admin tools". In order to use the Alexa Skill, I would say, "Alexa, open admin tools." Make sure to use the ARN captured above after you created your Lambda function.
<blockquote><strong>Tip: </strong>I wasn't able to find any information about this in the Alexa Skills Kit documentation but the skill never seemed to work when I included "AWS" or "Amazon" in the invocation name.</blockquote>
<h3>Interaction Model</h3>
This is where we tell the Alexa Skills Kit what our skill can do. In the "Intent Schema" box, paste the contents of the <a href="https://github.com/1Strategy/alexa-aws-administration/blob/master/intents.json">intents.json</a> file. Next, add a new slot type, giving the new slot a type name of "LIST_OF_REGIONS" and paste the contents of <a href="https://github.com/1Strategy/alexa-aws-administration/blob/master/intents.json">LIST_OF_REGIONS.txt</a> into the value field. Paste the contents of <a href="https://github.com/1Strategy/alexa-aws-administration/blob/master/sample_utterances.txt">sample_utterances.txt</a> into the "Sample Utterances" field and click next.
<h3>Test</h3>
You'll be presented with a screen that you can use to test example inputs and see what the skill returns. It can take a few seconds for the Alexa Skills Kit to build the model for use on your Echo. As soon as you see three green checkmarks on the left side, you should be good to test it out on your Echo!

# LICENSE

Code released under [the MIT license](https://github.com/1Strategy/alexa-aws-administration/blob/master/LICENSE).
