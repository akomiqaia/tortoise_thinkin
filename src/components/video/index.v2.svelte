
<!-- 
<script>
    var session = OT.initSession(apiKey, sessionId);
var publisher = OT.initPublisher('publisher', {
  insertMode: 'append',
  width: '100%',
  height: '100%'
});
var archiveID = null;

function disableForm() {
  $('.archive-options-fields').attr('disabled', 'disabled');
}

function enableForm() {
  $('.archive-options-fields').removeAttr('disabled');
}

function positionStreams() {
  var $focusElement;
  $focusElement = $('.focus');
  if ($('#streams').hasClass('vertical')) {
    $('#streams').children().css('top', '0');
    $focusElement.appendTo('#streams');
    $focusElement.css('top', (-20 * ($('#streams').children().size() - 1)) + '%');
  }
  else {
    $focusElement.prependTo('#streams');
    $focusElement.css('top', '0');
  }
}

function setFocus(focusStreamId) {
  var $focusElement;
  var otherStreams = $.map($('#streams').children(), function (element) {
    var streamId = (element.id === 'publisher' && publisher.stream) ? publisher.stream.streamId
      : element.id;
    if (streamId !== focusStreamId) {
      $('#' + element.id).removeClass('focus');
      return streamId;
    }
    return null;
  });

  $.post('/focus', {
    focus: focusStreamId,
    otherStreams: otherStreams
  }).done(function () {
    console.log('Focus changed.');
  }).fail(function (jqXHR, textStatus, errorThrown) {
    console.error('Stream class list error:', errorThrown);
  });

  $('.focus').removeClass('focus');
  $focusElement = (publisher.stream && publisher.stream.streamId === focusStreamId) ?
    $('#publisher') : $('#' + focusStreamId);
  $focusElement.addClass('focus');
  session.signal({
    type: 'focusStream',
    data: focusStreamId
  });
  positionStreams();
}

function createFocusClick(elementId, focusStreamId) {
  $('#' + elementId).click(function () {
    setFocus(focusStreamId);
  });
}

if (layout === 'verticalPresentation') {
  $('#streams').addClass('vertical');
}

session.connect(token, function (err) {
  if (err) {
    alert(err.message || err); // eslint-disable-line no-alert
  }
  session.publish(publisher);
});

publisher.on('streamCreated', function () {
  createFocusClick(publisher.id, publisher.stream.streamId);
  positionStreams();
});

session.on('streamCreated', function (event) {
  var subscriber;
  var streamId = event.stream.streamId;
  var $streamContainer = $('<div></div>');
  $streamContainer.attr('id', event.stream.id);
  $('#streams').append($streamContainer);
  subscriber = session.subscribe(event.stream, streamId, {
    insertMode: 'append',
    width: '100%',
    height: '100%'
  });

  if (streamId === focusStreamId) {
    setFocus(streamId);
  }
  createFocusClick(subscriber.id, streamId);
  positionStreams();
});

session.on('streamDestroyed', function (event) {
  var $streamElem = $('#' + event.stream.id);
  if ($streamElem.hasClass('focus')) {
    setFocus(publisher.stream.streamId);
  }
  $streamElem.remove();
  positionStreams();
});

session.on('archiveStarted', function (event) {
  archiveID = event.id;
  console.log('ARCHIVE STARTED');
  $('.start').hide();
  $('.stop').show();
  disableForm();
});

session.on('archiveStopped', function () {
  archiveID = null;
  console.log('ARCHIVE STOPPED');
  $('.start').show();
  $('.stop').hide();
  enableForm();
});

$(document).ready(function () {
  $('.start').click(function () {
    var options = $('.archive-options').serialize();
    disableForm();
    $.post('/start', options)
      .fail(enableForm);
  }).prop('disabled', false);
  $('.stop').click(function () {
    $.get('stop/' + archiveID);
  });
  $('.toggle-layout').click(function () {
    var newLayoutClass;

    if ($('#streams').hasClass('vertical')) {
      $('#streams').removeClass('vertical');
    }
    else {
      $('#streams').addClass('vertical');
    }

    positionStreams();

    newLayoutClass = $('#streams').hasClass('vertical') ? 'verticalPresentation'
      : 'horizontalPresentation';

    if (archiveID) {
      $.post('archive/' + archiveID + '/layout', {
        type: newLayoutClass
      }).done(function () {
        console.log('Archive layout updated.');
      }).fail(function (jqXHR) {
        console.error('Archive layout error:', jqXHR.responseText);
      });
    }

    session.signal({
      type: 'layoutClass',
      data: newLayoutClass
    });
  });
});

</script> -->

<script>
    import { url } from '@sveltech/routify'

</script>

<div class="container bump-me">
    <a href={$url(`/rooms`)}>Show me ThinkIns</a>

    <div class="body-content">

      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">Host</h3>
        </div>
        <div class="panel-body">
          <div id="streams">
            <div id="publisher" class="focus"></div>
          </div>
        </div>
        <div class="panel-footer">
          <form class="archive-options">
              <fieldset class="archive-options-fields">
                  <div class="form-group">
                      <p class="help-block">Archive Options:</p>
                      <label class="checkbox-inline">
                          <input type="checkbox" name="hasAudio" checked> Audio
                      </label>
                      <label class="checkbox-inline">
                          <input type="checkbox" name="hasVideo" checked> Video
                      </label>
                  </div>

                  <div class="form-group">
                      <p class="help-block">Output Mode:</p>
                      <label class="radio-inline">
                          <input type="radio" name="outputMode" value="composed" checked> Composed
                      </label>
                      <label class="radio-inline">
                          <input type="radio" name="outputMode" value="individual"> Individual
                      </label>
                  </div>
              </fieldset>
          </form>
          <button class="btn btn-danger start" disabled>Start archiving</button>
          <button class="btn btn-success stop">Stop archiving</button>
          <button class="btn toggle-layout">Toggle layout</button>
        </div>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Instructions</h3>
      </div>
      <div class="panel-body">
        <p>
          Click <strong>Start archiving</strong> to begin archiving this session.
          All publishers in the session will be included, and all publishers that
          join the session will be included as well.
        </p>
        <p>
          Click <strong>Stop archiving</strong> to end archiving this session.
          You can then go to <a href="/history">past archives</a> to
          view your archive (once its status changes to available).
        </p>
        <p>
          Click <strong>Toggle layout</strong> to toggle the layout
          between a vertical and horizontal presentation. The layout changes in all clients
          and (in a composed archive) in the archive.
        <p>
          Click any stream to set it to be the focus stream in the archive layout.
      </div>
    </div>
  </div>