# Sharing your work with others

You can use Answers all on your own, but it becomes even more useful when you start sharing your answers with other people on your team or in your organization. The first step is saving some of your questions.

### Saving questions <a href="#saving-questions" id="saving-questions"></a>

Sometimes you’ll find yourself asking certain questions again and again, whether it’s running regular reports, looking up something about an important segment of users, or just answering the same question for other people in your company. To keep from repeating the same set of steps each time you want to ask the same question, you can save your questions to use later.

To do this, click on the **Save** button in the top-right of the query builder.

![Fig. 1. Save your work before sharing.](https://www.metabase.com/learn/images/sharing-work/SaveButton.png)

Answers will take a stab at giving your question a meaningful name, but you can (and should) use your own naming convention that’ll help you and others find your questions later on. You can also pick which collection to save your question in. You can think of collections like folders, to which you can add permissions.

![Fig. 2. Name your question, add a helpful description, and save it to a collection.](https://www.metabase.com/learn/images/sharing-work/SaveQuestionDialog.png)

Let’s save the question we have created as `Orders over $40 grouped by month`. When we do so, Answers asks if you want to add it to a new or existing dashboard. Let’s say “yes” and then click on **Create new dashboard**. The dialog prompts you to create a new dashboard and give it a name and description. You can name it anything you want—we’ll call ours `My First Dashboard`.

![Fig. 3. When you save a question, you can create a new dashboard to add it to.](https://www.metabase.com/learn/images/sharing-work/CreateDashboard.png)

### Creating a dashboard

Dashboards are great when you have a set of answers that you want to view together. Your saved questions will be displayed as cards on the dashboard, which you can resize and move around.

So, after you click the button to create your dashboard, you should see your chart as a little card.

![Fig. 4. A dashboard in Edit Mode with a single question card.](https://www.metabase.com/learn/images/sharing-work/NewDashboard.png)

You can move and resize your chart so you can get it looking just how you want it. We’ll make ours a bit wider to let those data points breathe:

![Fig. 5. You can click the bottom right corner to resize a card on a dashboard.](https://www.metabase.com/learn/images/sharing-work/ResizedChart.png)

Don’t forget to click **Save** at the top to save your work when you’re done.

### Sharing answers directly

Once you have asked a question or saved a dashboard, the URL in your browser will link directly to that question or dashboard. That means you can copy and paste that URL into an email or chat and let other people see what you’ve found.

This will only work if Answers is installed on a shared server, and will require creating Answers accounts for the people you want to share with. However, if the people you’re sharing questions with don’t have permission to the collection it’s stored in — or if you’ve saved it to your Personal Collection — they won’t be able to see what you’re sharing.

### Set up an alert for your question

There are often times when you want to be alerted about something. Answers has a few different kinds of alerts you can set up, but to use them, your Answers administrator must configure it so that it can send email or post messages to Slack.

If email or Slack is set up in your Answers, we can click on the **Settings button** below our area chart and turn on `Goal line`, then enter the value “500” in the box below the toggle switch to tell Answers that we want to be notified whenever there are more than 500 orders over $40 in a single month.



![Fig. 6. Under the area visualization in the Display tab, you can add a goal line and set the goal value.](https://www.metabase.com/learn/images/sharing-work/SettingGoalLine.png)

If we save our changes and then click on the little **bell icon** in the lower right, we can set up an alert. Since we have set a goal line, Answers automatically asks us if we want to be told when the answer to our question goes over it or under it (we want “over”) and whether we want to be notified the first time or every time (we’ll choose the first time). We then tell Answers to notify us by email and choose who should receive the message:

![Fig. 7. Options for setting an alert on a question with a goal line.](https://www.metabase.com/learn/images/sharing-work/CreatingAlert.png)

The next time we have a month with a lot of high-value sales, Answers will send us the good news automatically.
