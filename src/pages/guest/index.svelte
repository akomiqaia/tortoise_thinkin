<script>
//     /* global OT, apiKey, sessionId, token, $, layout, focusStreamId */
// var session = OT.initSession(apiKey, sessionId);
// var publisher;

// var container = $('<div id = "publisher"></div>');

// if (layout === 'verticalPresentation') {
//   $('#streams').addClass('vertical');
// }

// container.addClass('focus');
// $('#streams').append(container);

// publisher = OT.initPublisher('publisher', {
//   insertMode: 'append',
//   width: '100%',
//   height: '100%'
// });

// function positionStreams() {
//   var $focusElement = $('.focus');
//   if ($('#streams').hasClass('vertical')) {
//     $focusElement.appendTo('#streams');
//     $('#streams').children().css('top', '0');
//     $focusElement.css('top', (-20 * ($('#streams').children().size() - 1)) + '%');
//   }
//   else {
//     $focusElement.prependTo('#streams');
//     $focusElement.css('top', '0');
//   }
// }

// function focusStream(streamId) {
//   var focusStreamId = streamId;
//   var $focusElement = (publisher.stream && publisher.stream.id === focusStreamId) ? $('#publisher')
//     : $('#' + focusStreamId);
//   $('.focus').removeClass('focus');
//   $focusElement.addClass('focus');
//   positionStreams();
// }

// session.connect(token, function (err) {
//   if (err) {
//     alert(err.message || err); // eslint-disable-line no-alert
//   }
//   session.publish(publisher);
// });

// session.on('streamCreated', function (event) {
//   var streamId = event.stream.id;
//   container = document.createElement('div');
//   container.id = streamId;
//   $('#streams').append(container);
//   session.subscribe(event.stream, streamId, {
//     insertMode: 'append',
//     width: '100%',
//     height: '100%'
//   });
//   if (streamId === focusStreamId) {
//     focusStream(streamId);
//   }
//   positionStreams();
// });

// session.on('streamDestroyed', function (event) {
//   $('#' + event.stream.id).remove();
//   positionStreams();
// });

// session.on('signal:layoutClass', function (event) {
//   if (event.data === 'horizontalPresentation') {
//     $('#streams').removeClass('vertical');
//     $('.focus').prependTo('#streams');
//   }
//   else {
//     $('#streams').addClass('vertical');
//     $('.focus').appendTo('#streams');
//   }
//   positionStreams();
// });

// session.on('signal:focusStream', function (event) {
//   focusStream(event.data);
// });

</script>
  <div class="container bump-me">

    <div class="body-content">

      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">Participant</h3>
        </div>
        <div class="panel-body">
          <div id="streams"></div>
        </div>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Instructions</h3>
      </div>
      <div class="panel-body">
          <table class="table">
            <thead>
              <tr>
                <th>When</th>
                <th>You will see</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td style="vertical-align: middle;">Archiving is started</td>
                <td><img src="img/archiving-on-message.png"></td>
              </tr>
              <tr>
                <td style="vertical-align: middle;">Archiving remains on</td>
                <td><img src="img/archiving-on-idle.png"></td>
              </tr>
              <tr>
                <td style="vertical-align: middle;">Archiving is stopped</td>
                <td><img src="img/archiving-off.png"></td>
              </tr>
            </tbody>
          </table>
      </div>
    </div>
  </div>