# Orchestrating Lambda Functions Using AWS Step Functions
 
## Introduction
 
AWS Lambda functions can be integrated within Step Functions state machines to orchestrate workflow and create conditional logic. In this AWS hands-on lab, you’ll be building two Lambda functions and orchestrating the functions using Step Functions. Step Functions use numerical scores provided by students to determine the appropriate rewards to provide to the students by invoking the respective Lambda function. The orchestration of the workflow will rely on Step Functions calling the appropriate Lambda function depending on the inputted score.
 
## Solution
 
Log in to the AWS Management Console using the credentials provided on the lab instructions page. Make sure you're using the `us-east-1` Region.
 
### Set Up S3

1. Navigate to the S3 console by searching for and selecting **S3** in the top search bar, or selecting it from **Recently Visited**.
1. On the right, click **Create bucket**.
1. Under **Bucket name**, enter *guru-rewards-datafeed* followed by some characters to make it unique.
1. Leave the other settings at default, and click **Create bucket**.
1. Under **Buckets**, click on the **guru-rewards-datafeed** bucket.
1. Under **Objects**, click **Create folder**.
1. Under **Folder name**, enter *guru-premium-courses*.
1. Click **Create folder**.
1. Click **Create folder** again; under **Folder name**, enter *guru-premium-lessons*.
1. Click **Create folder**.

#### Populate the Buckets with CSV Files
1. To populate the buckets with the CSV files, navigate to the [GitHub repository](https://github.com/ACloudGuru-Resources/content-processing-serverless-data-aws-lambda/tree/main/Hands-On-Lab) and download the CSV files.
    > **Note:** If necessary, review the [How to Download a CSV from GitHub](https://www.gitkraken.com/learn/git/github-download#:~:text=Click%20on%20the%20file%20you,the%20file%2C%20and%20select%20Save%20.) page.
1. Click on the **guru-premium-courses** folder.
1. In the upper right corner, click **Upload**.
1. Next to **Files and folders**, click **Add files**.
1. Select the **premium-courses.csv** file, and click **Open**.
1. Click **Upload**.
1. In the upper right corner, click **Close**.
1. In the upper left breadcrumb trail, click on your bucket name.
1. Under **Objects**, click **guru-premium-lessons**.
1. In the upper right corner, click **Upload**.
1. Next to **Files and Folders**, click **Add files**.
1. Select the **premium-lessons.csv** file, and click **Open**.
1. Click **Upload**.

### Create the Lambda Functions

#### Create the First Lambda Function
1. In the search bar at the top, search for and select **Lambda**.
1. In the upper right corner, click **Create function**.
1. Under **Create function**, ensure **Author from scratch** is selected.
1. Under **Basic information**, set the following values:
   - **Function name**: Enter *UnlockPremiumCourses*.
   - **Runtime**: Select **Python 3.9**.
   - **Architecture**: Select **x86_64**.
1. Leave the other settings at default, and click **Create function**; this may take a few minutes.
1. Once the function is created, under **Code source** > **lambda_function**, delete the code and paste in the code found in the [GitHub repository](https://github.com/ACloudGuru-Resources/content-processing-serverless-data-aws-lambda/blob/main/Hands-On-Lab/code/unlock_premium_courses_lambda_function.py) under `unlock_premium_courses_lambda_function`.
1. Update the bucket name on line `4` to match your bucket name (e.g., guru-rewards-datafeed).
1. At the top, click **Deploy** to save the changes.
1. Toward the top, under **Function overview**, click **Layers**.
1. Next to **Layers**, click **Add a layer**.
1. Under **Choose a layer**, set the following values:
   - **Layer source**: Ensure **AWS layers** is selected.
   - **AWS layers**: Select **AWSSDKPandas-Python39**.
   - **Version**: Select the available version.
1. Click **Add**.
1. Under **Function overview**, on the right, copy the function ARN and paste it in a separate text file for use later.

#### Create the Second Lambda Function
1. In the upper left breadcrumb trail, click **Functions**.
1. In the upper right corner, click **Create Function**.
1. Under **Create function**, ensure **Author from scratch** is selected.
1. Under **Basic Information**, set the following values:
   - **Function name**: Enter *UnlockPremiumLessons*.
   - **Runtime**: Select **Python 3.9**.
   - **Architecture**: Select **x86_64**.
1. Leave the other settings at default, and click **Create function**; this may take a few minutes.
1. Toward the top, under **Function overview**, click **Layers**.
1. Next to **Layers**, click **Add a layer**.
1. Under **Choose a layer**, set the following values:
   - **Layer source**: Ensure **AWS layers** is selected.
   - **AWS layers**: Select **AWSSDKPandas-Python39**.
   - **Version**: Select the available version.
1. Click **Add**.
1. Under **Code source** > **lambda_function**, delete the code and paste in the code found in the [GitHub repository](https://github.com/ACloudGuru-Resources/content-processing-serverless-data-aws-lambda/blob/main/Hands-On-Lab/code/unlock_premium_lessons_lambda_function.py) under `unlock_premium_lessons_lambda_function.py`.
1. Update the bucket name on line `4` to match your bucket name.
1. At the top, click **Deploy** to save the changes.
1. Under **Function overview**, on the right, copy the function ARN and paste it in a separate text file for use later.


#### Grant Functions Access to S3
1. Click on the **Configuration** tab.
1. In the left navigation menu, under **General configuration**, click **Permissions**.
1. Under **Execution role**, click on the listed role name.
1. Next to **Permissions policies**, click on the **Add permissions** dropdown menu and select **Attach policies**.
1. In the upper right corner, click **Create policy**.
1. In the **Visual editor** tab, click on **Service** to expand the menu.
1. In the search bar, search for and select **S3**.
1. Expand the **Actions** menu. Under **Access level**, expand the **List** sub-menu, and select **ListBucket**.
1. Expand the **Read** sub-menu, and select **GetObject**.
1. Expand the **Resources** menu.
1. Next to **bucket**, click on the **Add ARN** hyperlink.
1. In the **Add ARN(s)** pop-up menu, next to **Bucket name**, enter the name of your bucket.
1. Copy your bucket name, then click **Add**.
1. Next to **object**, click on the **Add ARN** hyperlink and set the following values:
   - **Bucket name**: Paste in your bucket name.
   - **Object name**: Select **Any**.
1. Click **Add**.
1. Click **Next** until you get to **Create policy**.
1. Next to **Name**, enter *ReadS3RewardsDatafeed*; copy **ReadS3RewardsDatafeed** for later.
1. Click **Create policy**.

#### Attach Policies
1. In the left navigation menu under **Access management**, click **Roles**.
1. In the **Roles** search bar, enter *courses*.
1. Click on the **UnlockPremiumCourses** role.
1. Next to **Permissions policies**, click on the **Add permissions** dropdown menu and select **Attach policies**.
1. In the **Other permissions policies** search bar, paste in *ReadS3RewardsDatafeed* and press **Enter**.
1. Click on the checkbox next to **ReadS3RewardsDatafeed**.
1. Click **Attach policies**.
1. In the upper left breadcrumb trail, click **Roles**.
1. In the **Roles** search bar, enter *lesson*, and select the **UnlockPremiumLessons** role.
1. Next to **Permissions policies**, click on the **Add permissions** dropdown menu and select **Attach policies**.
1. In the **Other permissions policies** search bar, paste in **ReadS3RewardsDatafeed** and press **Enter**.
1. Click on the checkbox next to **ReadS3RewardsDatafeed**.
1. Click **Attach policies**.

### Set Up IAM Roles

1. In the left navigation menu, under **Access management**, click **Roles**.
1. In the upper right corner, click **Create role**.
1. Under **Trusted entity type**, ensure **AWS service** is selected.
1. Under **Use case**, beneath **Use cases for other AWS services**, click on the dropdown menu and select **Step Functions** (in the search bar, you can search *Step Functions*).
1. Underneath the **Step Functions** dropdown menu, click on the radio button next to **Step Functions**.
1. Click **Next**.
1. Under **Permissions policies**, click on the **+** icon next to **AWSLambdaRole** to confirm the Step Functions can invoke Lambda.
1. Click **Next**.
1. Under **Role details** > **Role name**, enter *StepFunctionsInvokeLambdaRole*.
1. Leave the other settings at default, scroll down, and click **Create role**.

### Set Up AWS Step Functions

1. In the search bar at the top, search for and select **Step Functions**.
1. Under **Get started** on the right, click **Get started**.
1. In the line of text under **Review Hello World example**, click on the **here** hyperlink.
1. Under **Choose authoring method**, select **Write your workflow in code**.
1. Under **Definition**, delete the code and paste in the code found in the [GitHub repository](https://github.com/ACloudGuru-Resources/content-processing-serverless-data-aws-lambda/blob/main/Hands-On-Lab/code/state-machine-code.json) under `state-machine-code.json`.
1. Scroll down to line `23`, and replace `Insert the ARN of your UnlockPremiumCoursesFunction` with the corresponding ARN you previously pasted in the separate text file.
1. On line `29`, replace `Insert the ARN of your UnlockPremiumLesson` with the corresponding ARN you previously pasted in the separate text file.
1. Click **Next**.
1. Under **Name**, enter *RewardsProcessorStateMachine*.
1. Under **Permissions**, select **Choose an existing role**.
1. Leave the other settings at default, and click **Create state machine**.

#### Test the Step Functions
1. In the upper right corner, click **Start execution**.
1. In the **Start execution** pop-up menu, under **Input**, update the following values:
   - Highlight and delete `Comment`, then type `Score`.
   - Highlight and delete `"Insert your JSON here"`, then type `30`.
1. Click **Start execution**.
1. Under **Graph view**, click on the **UnlockPremiumLessons** button.
1. To the right of **Graph view**, ensure the **Input and output** provides the correct output (i.e., the state machine triggers the Lambda function that returns the premium lessons).
1. In the upper right corner, click **New execution**.
1. In the **Start execution** pop-up menu, replace `30` with `80`.
1. Click **Start execution**.
1. Under **Graph view**, click on the **UnlockPremiumCourses** button.
1. To the right of **Graph view**, ensure the **Input and output** provides the correct output (i.e., the state machine triggers the Lambda function that returns the premium courses).

## Conclusion
 
Congratulations — you've completed this hands-on lab!
