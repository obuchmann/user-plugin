# Front-end user plugin

This plugin adds front-end user management features to [OctoberCMS](http://octobercms.com).

## Session component

The session component should be added to a layout that has registered users. It has no default markup.

### User variable

You can check the logged in user by accessing the **{{ user }}** Twig variable:

    {% if user %}
        <p>Hello {{ user.name }}</p>
    {% else %}
        <p>Nobody is logged in</p>
    {% endif %}

### Signing out

The Session component allows a user to sign out of their session.

    <a data-request="onLogout" data-request-data="redirect: '/good-bye'">Sign out</a>

### Page restriction

The Session component allows the restriction of a page or layout by allowing only signed in users, only guests or no restriction. This example shows how to restrict a page to users only:

    title = "Restricted page"
    url = "/users-only"

    [session]
    security = "user"
    redirect = "home"

The `security` property can be user, guest or all. The `redirect` property refers to a page name to redirect to when access is restricted.

## Account component

The account component provides a user sign in form, registration form, activation form and update form. To display the form:

    title = "Account"
    url = "/account/:code?"

    [account]
    redirect = "home"
    paramCode = "code"
    ==
    {% component 'account' %}

If the user is logged out, this will display a sign in and registration form. Otherwise, it will display an update form. The `redirect` property is the page name to redirect to after the submit process is complete. The `paramCode` is the URL routing code used for activating the user, only used if the feature is enabled.

## Reset Password component

The reset password component allows a user to reset their password if they have forgotten it.

    title = "Forgotten your password?"
    url = "/forgot-password/:code?"

    [resetPass]
    paramCode = "code"
    ==
    {% component 'resetPassword' %}

This will display the initial restoration request form and also the password reset form used after the verification email has been received by the user. The `paramCode` is the URL routing code used for resetting the password.

## Using a login name

By default October will expect the login name to be an email address. To switch to using a user defined login name, simply use a form that supplies a `login` input name instead of an `email` input name. For example, with the **Account** component attached, this sign in form can be used:

    <form data-request="onSignin">
        <label>Login</label>
        <input name="login" placeholder="Enter your login">

        <label>Password</label>
        <input name="password" type="password" placeholder="Enter your password">

        <button type="submit" >Sign in</button>
    </form>

And for the registration form, we simply add the login field:

    <form data-request="onRegister">
        <label>Full Name</label>
        <input name="name" type="text" placeholder="Enter your full name">

        <label>Email</label>
        <input name="email" type="email" placeholder="Enter your email">

        <label>Login</label>
        <input name="login" type="email" placeholder="Pick a login name">

        <label>Password</label>
        <input name="password" type="password" placeholder="Choose a password">

        <button type="submit" class="btn btn-default">Register</button>
    </form>

We can add any other additional fields here too, such as `phone`, `company`, etc.

## Error handling

### Flash messages
This plugin makes use of October's [`Flash API`](http://octobercms.com/docs/cms/markup#flash-messages). In order to display the error messages, you need to place the following snippet in your layout or page.

    {% flash %} 
    <div class="alert alert-{{ type == 'error' ? 'danger' : type }}">{{ message }}</div> 
    {% endflash %}
    
### AJAX errors
The User plugin displays AJAX error messages in a simple ``alert()``-box by default. However, this might scare non-technical users. You can change the default behavior of an AJAX error from displaying an ``alert()`` message, like this:

    <script>
    $(window).on('ajaxErrorMessage', function(event, message){
    
        // This can be any custom JavaScript you want
        alert('Something bad happened, mate, here it is: ' + message);
    
        // This will stop the default alert() message
        event.preventDefault();
    
    })
    </script>
