---
layout: default
title: Dodanie obsługi video Brightcove dla SlickSlider'a
---

# Dodanie obsługi video Brightcove dla SlickSlider'a

## JavaScript
```javascript
document.querySelectorAll('.carousel-with-brightcove-videos').forEach( (module) => {
    const container = module;
    const slider = jQuery(container).find('.timeline-content');
    const slides = container.querySelectorAll('.timeline-content-single');
    let isVideoPlaying = false;
    const videoControlsWithoutPlayButton = container.querySelectorAll(".vjs-control:not(.vjs-play-control.vjs-control)");

    slides.forEach((slide) => {
        slide.addEventListener('click', () => {
            if (!isVideoPlaying) {
                isVideoPlaying = true;
                slider.slick('slickPause');
            } else {
                isVideoPlaying = false;
                slider.slick('slickPlay');
            }
        });
    });

    videoControlsWithoutPlayButton.forEach((control) => {
        control.addEventListener("click", (event) => {
            event.stopPropagation()
        }, false);
    });


    slider.on('beforeChange', () => {
        const currentPlayingVideo = jQuery(container).find('.vjs-playing video');

        if (0 !== currentPlayingVideo.length) {
            currentPlayingVideo.get(0).pause();
            slider.slick('slickPlay');
        }
    });
});
```

## SASS
```sass
.carousel-with-brightcove-videos {
    &.elementor-section.elementor-element {
        @include desktop-only {
            padding-bottom: 80px;
        }
    }

    & .elementor-column-wrap .elementor-widget-wrap {
        .timeline-content {
            padding: 0 30px;

            @include not-desktop {
                padding: 0;
            }

            & .slick-list {
                margin-bottom: 25px;
            }

            & .slick-dots {
                display: flex;
                flex-direction: row;

                & li .advanced-timeline-widget-item {
                    display: none;
                }
            }
        }
    }

    & .video-js {
        & .vjs-control-bar {
            background: #0099cc;
        }

        & .vjs-big-play-button {
            // stylelint-disable-next-line declaration-property-value-allowed-list
            background-color: transparent; // cant change it in another method cause its reset not our module (brightcove video styles)

            & .vjs-icon-placeholder::before {
                content: "";
                background-image: url("url-for-img");
                background-repeat: no-repeat;
                background-size: 46px;
                background-position: 55% calc(50% - 0px);
                border: none;
                box-shadow: none;
            }
        }

        & .vjs-time-tooltip, .vjs-volume-tooltip {
            width: max-content;
        }
    }

    .elementor-widget-advanced-timeline.kts-display-navigation-yes .box-arrow {
        @include not-desktop {
            // kurtosys advanced timeline don't allow set arrows for only desktop or mobile also use !important to set flex for it
            // stylelint-disable-next-line declaration-no-important
            display: none !important;
        }
    }
}

.elementor-editor-active .carousel-with-brightcove-videos .video-js.vjs-16-9:not(.vjs-audio-only-mode) {
    width: 200px;
    height: 100%;
}
```