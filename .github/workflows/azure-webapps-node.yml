// ====================
// AdBlock-Friendly Video Player Script
// ====================

const AwsIndStreamDomain = 'https://sunny387vks.com'; // মূল ভিডিও ডোমেইন, পরে নিজের ডোমেইনে পরিবর্তন করতে পারো

(function() {

    const AwsIndStreamIframeParamTr = IndStreamPlayerConfigs.tr !== false && IndStreamPlayerConfigs.tr > 0 ? '?tr=' + parseInt(IndStreamPlayerConfigs.tr) : '';
    const AwsIndStreamPlayerIframe = document.createElement('iframe');
    const AwsIndStreamIframeUrl = `${AwsIndStreamDomain}/play/${IndStreamPlayerConfigs.src}${AwsIndStreamIframeParamTr}`;
    var initIndStreamPlayer = false;

    // ====================
    // AdBlock Detection
    // ====================
    function detectAdBlock(callback) {
        let ad = document.createElement('div');
        ad.innerHTML = '&nbsp;';
        ad.className = 'adsbox';
        ad.style.position = 'absolute';
        ad.style.height = '1px';
        ad.style.width = '1px';
        ad.style.top = '-1000px';
        document.body.appendChild(ad);

        window.setTimeout(function() {
            if (ad.offsetHeight === 0) {
                callback(true); // AdBlock detected
            } else {
                callback(false); // No AdBlock
            }
            ad.remove();
        }, 100);
    }

    // ====================
    // Generate Player
    // ====================
    const genAwsPlayer = () => {
        AwsIndStreamPlayerIframe.setAttribute('src', AwsIndStreamIframeUrl);
        AwsIndStreamPlayerIframe.setAttribute('width', IndStreamPlayerConfigs.width);
        AwsIndStreamPlayerIframe.setAttribute('height', IndStreamPlayerConfigs.height);
        AwsIndStreamPlayerIframe.setAttribute('frameborder', 0);
        AwsIndStreamPlayerIframe.setAttribute('allowfullscreen', 'allowfullscreen');

        const AwsIndStreamPlayerContainer = typeof IndStreamPlayerConfigs.selector == 'string'
            ? document.querySelector(IndStreamPlayerConfigs.selector)
            : document.getElementById(IndStreamPlayerConfigs.id);

        if (AwsIndStreamPlayerContainer != null) {
            if (AwsIndStreamPlayerContainer.querySelector('iframe') == null) {
                AwsIndStreamPlayerContainer.appendChild(AwsIndStreamPlayerIframe);
            }
        } else {
            setTimeout(genAwsPlayer, 100);
        }
    };

    // ====================
    // Check Player URL
    // ====================
    const AwsIndStreamAjax = (url, success, error) => {
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = function() {
            if (xhr.status != 200) {
                if (typeof error == 'function') error();
            } else {
                if (xhr.readyState == XMLHttpRequest.DONE && typeof success == 'function') success();
            }
        }
        xhr.open('GET', url, true);
        xhr.send(null);
    };

    // ====================
    // Initialize Player
    // ====================
    detectAdBlock(function(isBlocked) {
        if (isBlocked) {
            // AdBlock detected – show overlay message
            const container = document.getElementById(IndStreamPlayerConfigs.id);
            if(container){
                container.innerHTML = '<div style="color:#fff;background:#000;text-align:center;padding:20px;">Please disable AdBlock to watch the video.</div>';
            }
            return; // stop loading player
        }

        if ('btn' in IndStreamPlayerConfigs && document.querySelector(IndStreamPlayerConfigs.btn) != null) {
            AwsIndStreamAjax(AwsIndStreamIframeUrl, () => {
                document.querySelector(IndStreamPlayerConfigs.btn).style.display = 'block';
                if ('success' in IndStreamPlayerConfigs && typeof IndStreamPlayerConfigs.success == 'function') {
                    IndStreamPlayerConfigs.success();
                }
            }, () => {
                document.querySelector(IndStreamPlayerConfigs.btn).style.display = 'none';
                if ('error' in IndStreamPlayerConfigs && typeof IndStreamPlayerConfigs.error == 'function') {
                    IndStreamPlayerConfigs.error();
                }
            });

            document.querySelector(IndStreamPlayerConfigs.btn).addEventListener('click', genAwsPlayer);
        } else {
            document.addEventListener('DOMContentLoaded', genAwsPlayer);
        }
    });

    // ====================
    // Message Listener (iframe events)
    // ====================
    function listener(event) {
        if ('origin' in event) {
            if (event.origin == AwsIndStreamDomain && !initIndStreamPlayer) {
                if ('event' in event.data) {
                    if (event.data.event == 'init') {
                        AwsIndStreamPlayerIframe.width = IndStreamPlayerConfigs.width;
                        AwsIndStreamPlayerIframe.height = IndStreamPlayerConfigs.height;
                        initIndStreamPlayer = true;
                    } else if (event.data.event == 'error') {
                        const container = document.getElementById(IndStreamPlayerConfigs.id);
                        if(container) container.remove();
                    }
                }
            }
        }
    }

    if (window.addEventListener) {
        window.addEventListener("message", listener);
    } else {
        window.attachEvent("onmessage", listener);
    }

})();
