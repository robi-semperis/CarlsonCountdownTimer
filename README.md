# CarlsonCountdownTimer
Counting down Carlson's last days


<!-- SharePoint Countdown Timer (Days : Hours : Minutes : Seconds) -->
<div id="sp-countdown" style="font-family: Segoe UI, Arial, sans-serif; max-width: 820px; margin: 16px auto;">
  <!-- Title / Description (optional) -->
  <h2 style="margin: 0 0 8px; font-weight: 600; font-size: 24px; color: #323130;">
    Countdown to the Event
  </h2>
  <p style="margin: 0 0 16px; color: #605E5C; font-size: 14px;">
    We're counting down to our big day—stay tuned!
  </p>

  <!-- Timer Container -->
  <div aria-live="polite" aria-label="Time remaining" 
       style="display: flex; gap: 12px; align-items: stretch; flex-wrap: wrap;">
    <!-- Each time unit block -->
    <div class="unit">
      <div class="value" id="days">0</div>
      <div class="label">Days</div>
    </div>
    <div class="unit">
      <div class="value" id="hours">00</div>
      <div class="label">Hours</div>
    </div>
    <div class="unit">
      <div class="value" id="minutes">00</div>
      <div class="label">Minutes</div>
    </div>
    <div class="unit">
      <div class="value" id="seconds">00</div>
      <div class="label">Seconds</div>
    </div>
  </div>

  <!-- Optional call-to-action button -->
  <div style="margin-top: 16px;">
    <a href="#" id="cta-link" 
       style="display:inline-block; padding:10px 16px; background:#0078D4; color:#fff; 
              text-decoration:none; border-radius0px;
    background: #F3F2F1;
    border: 1px solid #E1DFDD;
    border-radius: 6px;
    text-align: center;
    padding: 14px 10px;
  }
  #sp-countdown .value {
    font-size: 36px;
    font-weight: 700;
    color: #201F1E;
    line-height: 1.2;
  }
  #sp-countdown .label {
    margin-top: 6px;
    font-size: 12px;
    letter-spacing: 0.5px;
    text-transform: uppercase;
    color: #605E5C;
  }

  /* Responsive tweaks */
  @media (max-width: 480px) {
    #sp-countdown .value { font-size: 30px; }
    #sp-countdown .unit { flex: 1 1 46%; }
  }
</style>

<script>
  (function() {
    "use strict";

    // === CONFIGURE YOUR EVENT HERE ===
    // Use ISO format with timezone to avoid ambiguity:
    // Example: '2026-02-15T18:00:00-07:00' (Feb 15, 2026 at 6:00 PM, Arizona time)
    // If you prefer UTC, use '2026-02-16T01:00:00Z'
    var TARGET_DATE = '2026-02-15T18:00:00-07:00';

    // Optional: set CTA link text & URL
    var CTA_TEXT = 'Add to calendar';
    var CTA_URL  = '#'; // e.g., link to a Teams meeting, event page, or ICS

    // Apply CTA settings
    var ctaLink = document.getElementById('cta-link');
    if (ctaLink) {
      ctaLink.textContent = CTA_TEXT;
      ctaLink.href = CTA_URL;
    }

    var target = new Date(TARGET_DATE).getTime();

    // DOM references
    var daysEl    = document.getElementById('days');
    var hoursEl   = document.getElementById('hours');
    var minutesEl = document.getElementById('minutes');
    var secondsEl = document.getElementById('seconds');

    // Helper: pad numbers to 2 digits
    function pad2(n) { return (n < 10 ? '0' : '') + n; }

    function updateCountdown() {
      var now = Date.now();
      var diff = target - now;

      if (diff <= 0) {
        // Event reached – show zeros or switch to Count Up behavior if desired
        daysEl.textContent    = '0';
        hoursEl.textContent   = '00';
        minutesEl.textContent = '00';
        secondsEl.textContent = '00';

        // Optionally: replace title/CTA on completion
        // document.querySelector('#sp-countdown h2').textContent = 'The event is live!';
        // ctaLink.textContent = 'Join now';
        // ctaLink.href = '<your live link>';

        return; // stop showing negative values; leave final state
      }

      var seconds = Math.floor(diff / 1000);
      var days    = Math.floor(seconds / 86400);
      seconds    -= days * 86400;
      var hours   = Math.floor(seconds / 3600);
      seconds    -= hours * 3600;
      var minutes = Math.floor(seconds / 60);
      seconds    -= minutes * 60;

      daysEl.textContent    = String(days);
      hoursEl.textContent   = pad2(hours);
      minutesEl.textContent = pad2(minutes);
      secondsEl.textContent = pad2(seconds);
    }

    // Initial paint & interval
    updateCountdown();
    var intervalId = setInterval(function() {
      updateCountdown();
      // Optional: clear interval after event to save cycles
      if ((target - Date.now()) <= 0) { clearInterval(intervalId); }
    }, 1000);
  })();
</script>
