# cloudtrail-eventbridge-sns-signin

Creating a CloudTrail Trail and EventBridge Alert for Console Sign-Ins

1. Create Multi-Region CloudTrail Trail
By default, CloudTrail displays the last 90 days worth of events that occur within your AWS Account. Let's quickly explore that then get to work on creating our custom CloudTrail Trail!

    Navigate to CloudTrail.
    Find and select Dashboard.
    Under the dashboard, you will see a pane titled Event history where you can view the last 90 days of captured events.
    Explore some of the events to get familiar with their format. (Hint: Look for some sign-in events if possible!).
    Find and select Trails on the left-hand menu.
    Select Create trail. This will bring you to the Choose trail attributes screen.
    Provide the following Trail name: OurTrail.
    For storage location select Create new S3 bucket.
    Under Trail log bucket and folder verify the bucket name has been populated.
    (Optional) Provide a prefix for your logs if desired.

2. Create CloudTrail-specific KMS Key

    Ensure that the checkbox for Log file SSE-KMS encryption is selected and enabled.
    Select the New KMS key radio button.
    For AWS KMS alias enter CloudTrailKey (This creates a new KMS key specific to encrypting our CloudTrail Trail logs).
    Under Additional settings ensure that Log file validation is enabled. (This enables you to leverage AWS to determine whether a log file was modified, deleted, or unchanged after AWS CloudTrail delivered it to the destination).
    (Optional) Provide tags if desired.
    Select Next.

Choose Log Events

    Under the Events pane, for Event type, select Management events.
    Under the Management events pane, for API activity, ensure you have selected Read and Write.
    Select Next.
    Review the details, and then select Create trail.

3. Create SNS Topic and Subscription

Set Topic Attributes

    Navigate to Amazon Simple Notification Service (SNS).
    Select Topics from the left menu.
    Select Create topic.
    Under the Details pane, for Type, select Standard.
    For Name, enter: ConsoleAlerts.
    Leave all other defaults, scroll to the bottom and select Create topic.

Create the Subscription

    Navigate to your recently created ConsoleAlerts topic.
    Find and select Subscription in the console window.
    Select Create subscription.
    Ensure the topic ARN is correct, and then select Email from the Protocol dropdown.
    Input your email, or a temporary email, within the Endpoint textbox.
    Select Create subscription.

Confirm the Subscription

    Navigate to your email inbox and wait for the SNS confirmation email.
    Within the email, click the Confirm subscription link.
    Verify the confirmation by looking under the Subscriptions tab on the SNS Topic overview window (It should say confirmed).

Test the Subscription

    Find and select Publish message under the topic dashboard screen.
    (Optional) Enter a subject.
    Under Message body select `Identical payload for all delivery protocols.
    Enter your test message in the Message body to send to the endpoint textbox.
    Select Publish message.

4. Setting up Amazon EventBridge

Let's work on putting it all together!
Create Amazon EventBridge Rule

    Navigate to Amazon EventBridge.

    Find and select Rules from the menu on the left.

    Ensure the default bus is selected and then select Create rule.

    Under Name enter: ConsoleSigninAlerts.

    (Optional) Enter a description.

    Ensure default is selected under Event bus.

    Make sure the toggle is on for Enable the rule on the selected event bus.

    Under Rule type select Rule with event pattern.

    Select Next.

    For Event source choose AWS events or EventBridge partner events.

    Under Sample events choose AWS Console Sign In via CloudTrail.

    Under Creation method choose Custom pattern (JSON editor).

    Paste the below code into the Event pattern section. Be sure to update both references of REPLACE_ME with your account ID. 
    <<<<<<<< i have code in file custome_patter >>>>>>>>>>>>>>>>>>>

    

Scroll back up to the Sample events section, find and select AWS Console Sign In via CloudTrail and copy the resulting code.

Switch Sample event type to Enter my own, then paste the copied sample code back into the code block. 16. Replace the default ID references with your account ID in four places: lines 6, 14 , 15, and 16.

Scroll back down to the Event pattern section and click Test pattern.

After you get a successful match message, click Next.

Select the Target

    Under the Target 1 pane, for Target types select AWS service.
    For AWS service, select SNS Topic within the dropdown.
    Under Topic find and select your ConsoleAlerts SNS Topic.
    Click Skip to Review and create.
    Select Create rule.

Test it out!

    Sign-out of your AWS account.
    Sign back in to your account as the cloud_user using the lab credentials and URL.
    You should receive an email containing the JSON of the captured event!

