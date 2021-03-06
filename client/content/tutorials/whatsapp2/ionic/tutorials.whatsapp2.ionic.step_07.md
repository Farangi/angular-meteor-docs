{{#template name="tutorials.whatsapp2.ionic.step_07.md"}}


In this step we gonna take care of the app's security and encapsulation, since we don't want the users to do whatever they want, and we don't want them to be able to see content which is unrelevant for them.

We gonna start by removing a Meteor package named `insecure`.

This package provides the client with the ability to run collection mutation methods. This is a behavior we are not intrested in since removing data and creating data should be done in the server and only after certain validations.

Meteor includes this package by default only for development purposes and it should be removed once our app is ready for production.

So let's remove this package by running this command:

    $ meteor remove insecure

With that we're able to add ability to remove chats:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.2"}}

We have a Method, now we have to implement it in the UI.

Each chat has two buttons, one for sending attachments and one to open options menu.

Let's create that menu by creating a new component called `MessagesOptionsComponent`:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.3"}}

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.4"}}

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.5"}}

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.6"}}

Great! Now we can define component's method to open the options:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.7"}}

One thing missing, add this method to the view:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.8"}}

Right now all the chats are published to all the clients which is not very good for privacy. Let's fix that.

First thing we need to do in order to stop all the automatic publication of information is to remove the `autopublish` package from the Meteor server:

    $ meteor remove autopublish

Now we need to explicitly define our publications. Let's start by sending the users' information:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.10"}}

And add the messages:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.11"}}

We will now add the [publish-composite](https://atmospherejs.com/reywood/publish-composite) package which will help us implement joined collection pubications.

    $ meteor add reywood:publish-composite

Use `Meteor.publishComposite` from the package we installed and create a publication of `Chats`: 

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.13"}}

The chats publication is a composite publication which is made of several nodes. First we gonna find all the relevant chats for the current user logged in. After we have the chats, we gonna return the following cursor for each chat document we found. First we gonna return all the last messages, and second we gonna return all the users we're currently chatting with.

Those publications are still not visible by server, we need to import and run the init method:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.14"}}

Let's add the subscription for the chats publication in the chats component:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.15"}}

The users publication publishes all the users' profiles, and we need to use it in the new chat dialog whenever we wanna create a new chat.

Let's subscribe to the users publication in the new chat component:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.16"}}

The messages publication is responsible for bringing all the relevant messages for a certain chat. This publication is actually parameterized and it requires us to pass a chat id during subscription.

Let's subscribe to the messages publication in the messages component, and pass the current active chat id provided to us by the nav params:

{{> DiffBox tutorialName="whatsapp2-ionic-tutorial" step="7.17"}}

{{/template}}