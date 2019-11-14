# Mobile-REM
移动端适配

window.onload = function () {
    document.addEventListener('touchstart', function (event) {
      if (event.touches.length > 1) {
        event.preventDefault();
      }
    });
    var lastTouchEnd = 0;
    document.addEventListener('touchend', function (event) {
      var now = (new Date()).getTime();
      if (now - lastTouchEnd <= 300) {
        event.preventDefault();
      }
      lastTouchEnd = now;
    }, false);
    document.addEventListener('gesturestart', function (event) {
      event.preventDefault();
    });
    (function (doc, win) {
      var _rootFontSize = window._rootFontSize || 20;
      var _remMetaScalable = typeof window._remMetaScalable === 'undefined'
        ? false
        : !!window._remMetaScalable;

      var docEl = doc.documentElement,
        isIOS = navigator.userAgent.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/),
        dpr = isIOS ? Math.min(win.devicePixelRatio, 3) : 1,
        scale = 1 / dpr,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize';
      docEl.dataset.dpr = dpr;
      dpr = window.top === window.self ? dpr : 1;

      var metaEl = doc.createElement('meta');
      metaEl.name = 'viewport';
      var metaElContent = 'width=device-width, ';
      if (_remMetaScalable) {
        metaElContent += 'initial-scale=' + scale;
      } else {
        metaElContent += (
          'initial-scale=' + scale
          + ',maximum-scale=' + scale
          + ', minimum-scale=' + scale
          + ', user-scalable=no');
      }
      metaEl.content = metaElContent;
      docEl.firstElementChild.appendChild(metaEl);
      var recalc = function () {
        var width = docEl.clientWidth;
        docEl.style.fontSize = _rootFontSize * (width / 750) + 'px';
      };
      recalc();
      if (!doc.addEventListener) return;
      win.addEventListener(resizeEvt, recalc, false);
    })(document, window);

    document.body.style.visibility = 'visible'
  }
