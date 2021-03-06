Subscribe Email is a standalone UMD JavaScript module for rendering a mailing list sign up form quickly on a webpage.

It allows developers to quickly include an email collection form on a page without being concerned with the implementation details of a specific mailing list platform. It currently supports mailing lists on SendGrid, MailChimp and Universe.

# Getting the Module
You can get the module in any one of the following ways;
- Download [the latest release](https://github.com/blocks/subscribe-email/releases) from GitHub
- Or install with npm; `npm install subscribe-email`
- Or install with Bower; `bower install subscribe-email`

# Example Usage
To get started, you'll need to include the script on your page, create a placeholder element, and initialize the module. After you include subscribe-email.js in your project, here's some minimal code you can use to get started quickly;

```
<div id="subscribe-form"></div>
```


```
<script>
window.onload = function() {
  var mySubscribeForm = new SubscribeEmail({
    element: '#subscribe-form',
    service: 'universe',
    key: 'your-api-key-here'
  });
};
</script>
```

At a minimum, you'll need to change the `service` and `key` parameters to match your needs. *(Note: MailChimp uses `url` instead of `key`).*

# Advanced Usage

## Options
The module can be configured with several optional parameters passed to it's constructor. Here is the full list of options:

### `element`
**(Required)** A DOM element, jQuery element, or selector string to refer to the placeholder element.

### `alerter`
SubscribeEmail uses [`blocks-alerter`](https://github.com/blocks/alerter) to display response messages from the different mailing list platforms. By default, alerts will be prepended to `element` and use Alerter's default template. You can turn alerts off by setting `alerter: false`. If you would like to override the defaults, you can pass in an options hash with [options for Alerter](https://github.com/blocks/alerter#options). Example;

```
var mySubscribeForm = new SubscribeEmail({
  element: '#subscribe-form',
  service: 'universe',
  key: 'your-api-key-here',
  alerter: {
    prependTo: 'body',
    template: myCustomTemplate
  }
});
```

### `service`
**(Required)** The mailing list platform you are using. Available options are `mailchimp`, `sendgrid` and `universe`.

### `key`
**(Required)** A string of the API key for your mailing list platform. *(This is not required for MailChimp. Instead, you'll have to use `url`).*

### `url`
**(Required for MailChimp)** A string of the `<form action="">` attribute generated by MailChimp which contains MailChimp authentication information. You can get this from MailChimp under "Signup forms > Embedded form code > Naked" and copying just the value from the `<form action="">` attribute. It should follow this format: `http://{username}.{data center}.list-manage.com/subscribe/post?u={user id}&id={list id}`.

### `submitText`
A string to be used on the form's submit button (defaults to "Subscribe").

### `template`
If you want to customize the markup, you can override the default markup by passing in a *compiled* handlebars template using this option. See the default template for a starting point to work from. A custom template will not work without a form tag that contains `id="{{id}}" data-subscribe-instance="{{instanceId}}"` and an email input that contains `name="{{emailName}}"`. (Defaults to `false`).

### `namespace`
Out of the box, the module will generate BEM markup with the namespace `subscribe-email`, but you can use this option to override the default without passing in a custom template.

## Events
You can easily integrate the messages into other parts of your page by listening for events being emitted from the SubscribeEmail instance;

```
mySubscribeForm.on('subscriptionMessage', function(payload){
  console.log(payload);
});
```

You can listen for the following events;

### `subscriptionMessage`
Fires whenever the mailing list provider returns a response (both success and failure). The message will be passed to this event as a string.

### `subscriptionError`
This event will fire if the mailing list provider returns an error. Specific details about the error will be passed to the event as a payload object. Note: the payload object may vary depending on the service.

### `subscriptionSuccess`
This event will fire if the mailing list provider returns a confirmation that the email address has been added to the list. Specific details will be passed to the event as a payload object. Note: the payload object may vary depending on the service.
