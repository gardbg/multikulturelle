var $stars;
var maxRequestNumber = 5;
var requestCounter = 0;
var $rateTooltip;
var $popovers;

jQuery(document).ready(function ($) {
  if ($.fn.tooltip.Constructor.Default.whiteList) {
    var myDefaultWhiteList = $.fn.tooltip.Constructor.Default.whiteList
    myDefaultWhiteList.textarea = [];
    myDefaultWhiteList.button = [];
  }

  var clickedStarIndex;
  var pageType;

  $stars = $('.stars');
  $popovers = $('.popovers');
  $rateTooltip = $('#rateTooltip');

  $popovers.attr('data-content', "<div class='md-form my-0 py-0'> <textarea type='text' style='font-size: 0.78rem' class='md-textarea form-control py-0' placeholder='Write us what can we improve' id='voteDescriptionInput' rows='3'></textarea> <button id='voteSubmitButton' type='submit' class='btn btn-sm btn-primary'>Submit!</button> <button id='closePopoverButton' class='btn btn-flat btn-sm'>Close</button>  </div>");

  $stars.on('mouseover', function () {
    pageType = $(this).attr('data-page-template');
  });

  $stars.on('click', function () {
    clickedStarIndex = $(this).attr('data-index');

    markStarsAsActive(clickedStarIndex);
    submitVote('');
  });

  $('#rateTooltip').on('mousedown', '#voteSubmitButton', function () {
    var voteDescription = $('#voteDescriptionInput').val();
    submitVote(voteDescription);
    $popovers.popover('hide');
  });

  $('#rateTooltip').on('click', '#closePopoverButton', function () {
    $popovers.popover('hide');
  });

  $stars.on('mouseover', function () {
    var index = $(this).attr('data-index');
    $popovers.attr('title', setPopoverTitle(index, pageType));
    $popovers.attr('data-original-title', setPopoverTitle(index, pageType));
    markStarsAsActive(index);
  });

  $stars.on('mouseleave', function () {
    unmarkActive();
    markStarsAsActive(clickedStarIndex);
  });

  $('.stars').on('click', function () {
    $('.stars').popover('hide');
  });
});

function markStarsAsActive(index) {
  unmarkActive();
  for (var i = 0; i <= index; i++) {
    $($stars.get(i)).addClass('amber-text');
  }
}

function unmarkActive() {
  $stars.removeClass('amber-text');
}

function setPopoverTitle(index, type) {
  var popoverTitleText = type === 'tutorial' ? 'lesson' : 'documentation';

  if (requestCounter >= maxRequestNumber) {
    return 'You have reached the vote limit';
  }

  switch (index) {
    case '0': {
      return 'Useless ' + popoverTitleText;
    }
    case '1': {
      return 'Poor ' + popoverTitleText;
    }
    case '2': {
      return 'Ok ' + popoverTitleText;
    }
    case '3': {
      return 'Good ' + popoverTitleText;
    }
    case '4': {
      return 'Excellent ' + popoverTitleText;
    }
  }
}

function submitVote(description) {
  doSubmit(description);
}


function getDevice() {
  if (/Mobi/.test(navigator.userAgent)) {
    return 1;
  }

  if (/Tablet|iPad/i.test(navigator.userAgent)) {
    return 2;
  }

  return 3;
}

function doSubmit(desc) {
  requestCounter++;
  if (requestCounter <= maxRequestNumber) {
    $.ajax({
      method: 'POST',
      url: example_ajax_obj.ajaxurl,
      data: {
        action: 'pages_rates_request',
        vote_value: $('.stars.amber-text').length,
        vote_description: desc,
        page_id: $rateTooltip.attr('data-page-id'),
        guest_id: localStorage.getItem('_uuid').substring(0, 5),
        device_type: getDevice()
      }
    }).fail(function (errorThrown) {
      toastr.clear();
      toastr["error"]("An error has occurred. Try again in a few moments.");
      console.log(errorThrown);
    }).done(function () {
      toastr.clear();
      toastr["success"]("Thank you for your feedback!");
    })
  }
}

$(function () {
  $('.stars').popover({
    container: '.popover-form-container'
  });
});
