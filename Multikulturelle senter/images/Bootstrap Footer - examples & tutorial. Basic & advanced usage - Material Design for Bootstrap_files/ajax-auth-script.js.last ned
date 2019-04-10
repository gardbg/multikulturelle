jQuery( function ( $ ) {

  var $currentForm = null;
  var $formLogin = $('form#login');
  var $formRegister =  $('form#register');

  $formRegister.validate({
    rules: {
      password2: {
        equalTo: '#signonpassword'
      }
    }
  });
  $formLogin.validate();

  $formLogin.on( 'submit', onSubmit );
  $formRegister.on( 'submit', onSubmit );

  function onSubmit(e) {

    e.preventDefault();

    $currentForm = $(this);

    if ( ! $currentForm.valid() ) return false;

    if ( $currentForm.is( $formRegister ) ) {

      register();
    } else {

      login();
    }
  }

  function register(e) {

    showStatus(ajax_auth_object.loadingmessage);

    var payload = {
      action: 'ajaxregister',
      name: $('#signonname').val(),
      username: $('#signonusername').val(),
      password: $('#signonpassword').val(),
      email: $('#email').val(),
      security: $('#signonsecurity').val()
    };
    doSubmit(payload);
  }

  function login(e) {

    showStatus(ajax_auth_object.loadingmessage);

    var payload = {
      action: 'ajaxlogin',
      username: $('#username').val(),
      password: $('#password').val(),
      email: '',
      security: $('#security').val(),
      name: ''
    };
    doSubmit(payload);
  }

  function doSubmit(data) {

    $.ajax({
      type: 'POST',
      dataType: 'json',
      url: ajax_auth_object.ajaxurl,
      data: data
    }).done( function ( response ) {

      showStatus(response.message);

      if (response.loggedin === true) {

        document.location.href = $currentForm.is( $formRegister ) ? 'https://mdbootstrap.com/registration-completed/' : window.location.href;
      }
    }).fail(console.error);
  }

  function showStatus(text) {

    $currentForm.find('p.status').show().text(text);
  }
});
