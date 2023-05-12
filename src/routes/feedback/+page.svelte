<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<style>
  /* Global Styles */
  body {
      padding-top: 30px;
      background-color: #4d4dff !important;
      overflow-x: hidden;
  }

  h2 {
      background-color: #4d4dff;
      text-align: left;
      font-family: "Lato", sans-serif;
      padding-bottom: 20px;
      padding-top: 0;
      color: #44d62c !important;
      font-weight: bold !important;
      font-size: 150% !important;
  }

  form {
        margin: 0 auto;
        padding: 50px;
        background-color: #4d4dff;
        border-radius: 12px;
        border: 2.5px solid #c724b1;
        padding-top: 10px;
        box-sizing: border-box; /* Include padding and border in the width calculation */
        max-width: 600px; /* Adjust the width of the form as needed */
        width: 150%; /* Add this line to widen the form */
        margin-top: 20px; /* Add this line to add spacing between the form and other elements */
    }

    @media screen and (max-width: 768px) {
        form {
            padding: 20px;
        }
    }

  input[type="email"],
  input[type="text"],
  textarea,
  select {
      width: 100%;
      padding: 12px;
      border: 1.5px solid #ddd;
      border-radius: 4px;
      box-sizing: border-box;
  }

  input[type="email"]:focus,
  input[type="text"]:focus,
  textarea:focus {
      border: 2px solid #c724b1;
  }

  label {
      font-weight: bold;
      display: block;
      margin-bottom: 10px;
      color: #44d62c;
      margin-top: 10px;
  }

  input[type="submit"] {
      display: block;
      margin: 0 auto;
      background-color: #44d62c;
      color: #fff;
      padding: 12px 50px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      margin-top: 10px;
  }

  input[type="submit"]:hover {
      background-color: #36AB23;
  }

  .modal-header {
      background-color: #c724b1;
      color: #fff;
      text-align: center;
      font-weight: bold;
      border-radius: 12px;
  }

  .modal-footer {
      background-color: #f5f5f5;
      border-radius: 12px;
  }

  .modal-footer .btn-default {
      background-color: #c724b1;
      color: #fff;
      border: none;
      border-radius: 12px;
  }

  .modal-footer .btn-default:hover {
      background-color: #722b86;
  }

  #loading {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      z-index: 9999;
  }

  .g-recaptcha{
        padding-top: 20px;
        padding-bottom: 10px;
        width: fit-content;
        margin: 0 auto;
    }
</style>



<svelte:head>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
  <script src="https://www.google.com/recaptcha/api.js" async defer></script>
  <script>
    $(document).ready(function() {
      $("form").submit(function(event) {
        event.preventDefault();
        var $submitButton = $('input[type="submit"]');
        var response = grecaptcha.getResponse();
        if (response.length == 0) {
          alert("Please verify that you are not a robot.");
          return false;
        } else {
          $submitButton.prop('disabled', true); // disable the submit button
          $('#loading').show();
          $.ajax({
            url: "https://script.google.com/macros/s/AKfycbw0k6AukzSvS69qmiUOWLJfv9xw4qO07YU5grw-ukvMZxCKvSxdH_XwU66cqGZDx2c/exec",
            method: "POST",
            dataType: "json",
            data: $("form").serialize(),
            success: function() {
                $submitButton.prop('disabled', false);
                $('#loading').hide();
                $('#feedback-form')[0].reset(); // reset the form
                $('#myModal .modal-title').html('Success');
                $('#myModal .modal-body').html('Your feedback has been submitted.');
                $('#myModal').modal('show'); // show the success modal
            },
            error: function() {
                $submitButton.prop('disabled', false);
                $('#loading').hide();
                $('#myModal .modal-title').html('Error');
                $('#myModal .modal-body').html('There was a problem submitting your feedback.');
                $('#myModal').modal('show'); // show the error modal
            }
          });
        }
      });
    });

    function onSubmit(token) {
      document.getElementById("feedback-form").submit();
    }
  </script>
</svelte:head>



<body>
	<form action="https://script.google.com/macros/s/AKfycbw0k6AukzSvS69qmiUOWLJfv9xw4qO07YU5grw-ukvMZxCKvSxdH_XwU66cqGZDx2c/exec" method="POST" id="feedback-form">
        <input type="hidden" name="g-recaptcha-response" id="g-recaptcha-response">        
        <h2>This form can be utilized to propose rule changes, report cheating/collusion or provide general feedback.</h2>
        <label for="name">Name:</label>
		<input type="text" name="name" required />
		<br />        
        <label for="email">Email:</label>
		<input type="email" name="email" required />
		<br />
		<label for="topic">Topic:</label>
		<select name="topic" required>
			<option value="">--Select a topic--</option>
			<option value="Rule Change">Rule Change</option>
			<option value="Site Feedback">Site Feedback</option>
			<option value="Suspected Cheating or Collusion">Suspected Cheating or Collusion</option>
			<option value="General Comment">General Comment</option>
			<option value="Other">Other</option>
		</select>
		<br />
		<label for="feedback">Feedback:</label>
		<textarea name="feedback" rows="5" cols="30" resize: vertical required></textarea>
		<br />
        
        <div class="g-recaptcha" data-sitekey="6LcKUN0lAAAAALl_apA-Fs6jXuEFkCtS7e-_e8cH"></div>
        <div style="text-align: center;">
		    <input type="submit" value="Submit" onclick="showLoading()"/>
        </div>
	</form>
    <div id="loading" style="display:none;">
        <svg width="128" height="128" viewBox="0 0 64 64">
          <circle cx="32" cy="32" r="28" stroke="#000" stroke-width="4" fill="none"/>
          <circle cx="32" cy="32" r="28" stroke="#000" stroke-width="2" fill="none">
            <animate attributeName="r" from="0" to="28" dur="1.5s" repeatCount="indefinite"/>
            <animate attributeName="opacity" from="1" to="0" dur="1.5s" repeatCount="indefinite"/>
          </circle>
        </svg>
      </div>        
	<div class="modal fade" id="myModal" role="dialog">
		<div class="modal-dialog">
			<div class="modal-content">
				<div class="modal-header">
					<button type="button" class="close" data-dismiss="modal">&times;</button>
				</div>
				<div class="modal-body">
				</div>
				<div class="modal-footer">
					<button type="button" class="btn btn-default" data-dismiss="modal">OK</button>
				</div>
			</div>
		</div>
	</div>
    <div class="modal fade" id="myModal" role="dialog">
        <div class="modal-dialog">
            <div class="modal-content">
                <div class="modal-header">
                    <button type="button" class="close" data-dismiss="modal">&times;</button>
                </div>
                <div class="modal-body"></div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
                </div>
            </div>
        </div>
    </div>
</body>

