---
layout: default
title: Video slider dla Wordpress z wykorzystaniem ACF
---

# Video slider dla Wordpress z wykorzystaniem ACF

## Struktura modułu
```php
declare(strict_types=1);

\defined('ABSPATH') || exit;

$customClasses = get_sub_field('centered_video_slider_custom_classes');
$title = get_sub_field('centered_video_slider_title');
$description = get_sub_field('centered_video_slider_description');
?>
<section class="centered-video-slider <?= $customClasses ?>">
    <div class="container">
        <?php if($title || $description) { ?>
            <div class="centered-video-slider__content-text">
                <?php if($title) { ?>
                    <h2 class="centered-video-slider__title"><?= $title ?> </h2>
                <?php } ?>
                <?php if($description) { ?>
                    <div class="centered-video-slider__description"><?= $description ?> </div>
                <?php } ?>
            </div>
        <?php } ?>
        <div class="centered-video-slider__element">
            <?php if( have_rows('centered_video_slider_slides') ) {
                while( have_rows('centered_video_slider_slides') ) {
                the_row();
                $videoLink = get_sub_field('centered_video_slider_slide_url');
                $videoID = get_vimeoid($videoLink);
                $image = get_sub_field('centered_video_slider_slide_thumbnail');
            ?>
                <div class="centered-video-slider__slide">
                    <div class="centered-video-slider__content-video">
                        <iframe tabindex="-1" class="centered-video-slider__player"
                                src="https://player.vimeo.com/video/<?= $videoID ?>?api=1"
                                width="920" height="520"
                                allow="autoplay">
                        </iframe>
                        <img class="centered-video-slider__placeholder"  src="<?= $image['url'] ?>" alt="<?= $image['alt'] ?>">
                        <div class="centered-video-slider__magic-trick"></div>
                        <button class="centered-video-slider__play" aria-label="Play"></button>
                    </div>
                </div>
            <?php }
            } ?>
        </div>
    </div>
</section>
```

## Dodanie odpowiedniego zachowania video przy zmianie slide'u
```javascript
const centeredVideosPlay = document.querySelectorAll('.centered-video-slider__play');

if(centeredVideosPlay.length) {
    centeredVideosPlay.forEach(button => {
        button.classList.add("centered-video-slider__play--show");
    });

    let paused = false;

    window.addEventListener("click", (e) => {
        const eTarget = e.target;

        if(eTarget.classList.contains("centered-video-slider__play")) {
            eTarget.closest(".centered-video-slider__content-video").classList.add("centered-video-slider__content-video--active-play")
            const playerIframe = eTarget.closest(".centered-video-slider__content-video").querySelector(".centered-video-slider__player");
            const player = new Vimeo.Player(playerIframe);
            playerIframe.removeAttribute("tabindex");
            player.play();
        }

        if(eTarget.classList.contains("centered-video-slider__magic-trick")) {
            const playerIframe = eTarget.closest(".centered-video-slider__content-video").querySelector(".centered-video-slider__player")
            const player = new Vimeo.Player(playerIframe);

            if(paused) {
                player.play();
                paused = false;
            } else {
                player.pause();
                paused = true;
            }
        }
    });
}
```

## Dodanie pól do ACF bezpośrednio w PHP
```php
'layout_63db86f782b10' => array(
    'key' => 'layout_63db86f782b10',
    'name' => 'centered_video_slider',
    'label' => 'Centered video slider',
    'display' => 'row',
    'sub_fields' => array(
        array(
            'key' => 'field_63db86f782b51',
            'label' => 'Custom classes',
            'name' => 'centered_video_slider_custom_classes',
            'type' => 'text',
            'instructions' => '',
            'required' => 0,
            'conditional_logic' => 0,
            'wrapper' => array(
                'width' => '',
                'class' => '',
                'id' => '',
            ),
            'default_value' => '',
            'placeholder' => '',
            'prepend' => '',
            'append' => '',
            'maxlength' => '',
        ),
        array(
            'key' => 'field_63db86f782b90',
            'label' => 'Section title',
            'name' => 'centered_video_slider_title',
            'type' => 'text',
            'instructions' => '',
            'required' => 0,
            'conditional_logic' => 0,
            'wrapper' => array(
                'width' => '',
                'class' => '',
                'id' => '',
            ),
            'default_value' => '',
            'placeholder' => '',
            'prepend' => '',
            'append' => '',
            'maxlength' => '',
        ),
        array(
            'key' => 'field_63dba7b38cae3',
            'label' => 'Section description',
            'name' => 'centered_video_slider_description',
            'type' => 'wysiwyg',
            'instructions' => '',
            'required' => 0,
            'conditional_logic' => 0,
            'wrapper' => array(
                'width' => '',
                'class' => '',
                'id' => '',
            ),
            'default_value' => '',
            'placeholder' => '',
            'prepend' => '',
            'append' => '',
            'maxlength' => '',
        ),
        array(
            'key' => 'field_63dbaae9129a1',
            'label' => 'Slides',
            'name' => 'centered_video_slider_slides',
            'type' => 'repeater',
            'instructions' => '',
            'required' => 0,
            'conditional_logic' => 0,
            'wrapper' => array(
                'width' => '',
                'class' => '',
                'id' => '',
            ),
            'collapsed' => '',
            'min' => 0,
            'max' => 0,
            'layout' => 'row',
            'button_label' => '',
            'sub_fields' => array(
                array(
                    'key' => 'field_63dbaae9129e0',
                    'label' => 'Thumbnail',
                    'name' => 'centered_video_slider_slide_thumbnail',
                    'type' => 'image',
                    'instructions' => '',
                    'required' => 0,
                    'conditional_logic' => 0,
                    'wrapper' => array(
                        'width' => '',
                        'class' => '',
                        'id' => '',
                    ),
                    'return_format' => 'array',
                    'preview_size' => 'thumbnail',
                    'library' => 'all',
                    'min_width' => '',
                    'min_height' => '',
                    'min_size' => '',
                    'max_width' => '',
                    'max_height' => '',
                    'max_size' => '',
                    'mime_types' => '',
                ),
                array(
                    'key' => 'field_63dbaae912a1c',
                    'label' => 'Vimeo link',
                    'name' => 'centered_video_slider_slide_url',
                    'type' => 'url',
                    'required' => 0,
                    'conditional_logic' => 0,
                    'wrapper' => array(
                        'width' => '',
                        'class' => '',
                        'id' => '',
                    ),
                    'default_value' => '',
                    'placeholder' => '',
                ),
            ),
        ),
    ),
    'min' => '',
    'max' => '',
),
```

## Some styles
```sass
.centered-video-slider {
    &__title {
        font-style: normal;
        font-weight: 300;
        font-size: 48px;
    }
    &__description {
        max-width: 730px;
        margin-left: auto;
        margin-right: auto;
    }

    &__title, &__description {
        text-align: center;
        padding-bottom: 48px;
    }

    &__player {
        border: none;
        border-radius: 16px;
    }

    &__placeholder {
        object-fit: cover;
    }

    &__placeholder,
    &__player {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
    }

    &__play,
    &__placeholder {
        opacity: 1;
        transition: opacity 250ms ease-in-out;
    }

    & &__play {
        display: none;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background-color: transparent;
        border: none;
        cursor: pointer;
        padding: 0;
        z-index: 3;

        &::before {
            content: '';
            display: block;
            width: 56px;
            height: 56px;
            background-size: cover;
            background-repeat: no-repeat;
            background-image: url('data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTA1IiBoZWlnaHQ9IjEwNSIgdmlld0JveD0iMCAwIDEwNSAxMDUiIGZpbGw9Im5vbmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+CjxwYXRoIGQ9Ik01Mi41IDEwNUMyMy41MDU0IDEwNSAwIDgxLjQ5NDYgMCA1Mi41QzAgMjMuNTA1NCAyMy41MDU0IDAgNTIuNSAwQzgxLjQ5NDYgMCAxMDUgMjMuNTA1NCAxMDUgNTIuNUMxMDUgODEuNDk0NiA4MS40OTQ2IDEwNSA1Mi41IDEwNVpNMzguMDY0IDMwLjg1MTRWNzUuOTI2M0w3Ni45MzU3IDUzLjM4ODhMMzguMDY0IDMwLjg1MTRaIiBmaWxsPSJ3aGl0ZSIvPgo8L3N2Zz4K');

            @media screen and (min-width: $breakpoint-sm) {
                width: 72px;
                height: 72px;
            }

            @media screen and (min-width: $breakpoint-md) {
                width: 105px;
                height: 105px;
            }
        }

        &--show {
            display: block;
        }
    }

    &__magic-trick {
        position: absolute;
        top: 8%;
        left: 0;
        width: 95%;
        height: 84%;
        z-index: 2;
    }

    &__content-video {
        padding-bottom: 56.523%; // = height 520px for 920px width
        position: relative;
        overflow: hidden;
        border-radius: 16px;
        background-color: $black;

        &::after {
            content: '';
            background-color: $black;
            opacity: 0.6;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            z-index: 2;
            transition: opacity 250ms ease-in-out;
        }

        &--active-play {
            .centered-video-slider__placeholder,
            .centered-video-slider__play,
            &.centered-video-slider__content-video::after {
                opacity: 0;
                visibility: hidden;
            }
        }
    }

    .slick-slider {
        .slick-slide {
            margin: 0 10px;
        }

        .slick-dots {
            position: initial;
            padding-top: 24px;

            > li {
                line-height: 0;
                display: inline-block;
            }

            > li:only-child {
                display: none;
            }

            button {
                background-color: $darkGrey;
                opacity: 0.5;
            }

            .slick-active {
                button {
                    opacity: 1;
                }
            }
        }
    }
}
```