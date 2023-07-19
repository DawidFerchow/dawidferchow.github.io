---
layout: default
title: Stylowanie slidera z użyciem SASS
---

# Stylowanie slidera z użyciem SASS

## SASS
```sass
$icon-width-desktop: 70px;
$icon-height-desktop: 70px;
$icon-offset-desktop: 30px;
$icon-offset-mobile: 40px;
$icon-width-mobile: 56px;
$icon-height-mobile: 56px;

.logo-carousel {
  &__heading,
  &__description {
    max-width: 564px;
    margin: 0 auto;
  }

  & .carousel {
    margin: 0;

    @include media-custom-max(1334px) {
      width: 85%;
      margin: 0 auto;
    }

    @include mobile-only {
      width: 450px;
      margin: 0 auto;
    }

    @include media-custom-max(450px) {
      width: 100%;
      margin: 0 auto;
    }
  }

  & .carousel-wrap {
    @include mobile-only {
      padding-bottom: calc(#{$icon-height-mobile} + #{$icon-offset-mobile});
    }
  }

  & .carousel__item {
    padding: 0 60px;

    @include tablet-only {
      padding: 0 30px;
    }

    @include mobile-only {
      padding: 0 25px;
    }
  }

  & .carousel .arrow {
    width: $icon-width-desktop;
    height: $icon-height-desktop;
    margin-top: calc(-#{$icon-height-desktop} / 2);
    z-index: 1;

    @include mobile-only {
      top: auto;
      bottom: calc(-#{$icon-offset-mobile} - #{$icon-height-mobile});
      width: $icon-width-mobile;
      height: $icon-height-mobile;
    }
  }

  $nameAndDirectionMap: ("prev": "left", "next": "right");

  @each $name, $direction in $nameAndDirectionMap {
    & .carousel .arrow.#{$name}-arrow {
      #{$direction}: calc(-#{$icon-width-desktop} - #{$icon-offset-desktop});

      @include media-custom-max(1334px) {
        #{$direction}: -#{$icon-width-desktop};
      }

      @include mobile-only {
        #{$direction}: calc(50% - (#{$icon-width-mobile} + (#{$icon-offset-mobile} / 2)));
      }

      &::before {
        content: url("data:image/svg+xml,%3Csvg viewBox='0 0 70 70' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0.810161 34.8303C0.810162 16.0414 16.0414 0.809947 34.8302 0.809949C53.6189 0.80995 68.8502 16.0414 68.8502 34.8303C68.8502 53.6192 53.6189 68.8506 34.8302 68.8506C16.0414 68.8506 0.810159 53.6192 0.810161 34.8303Z' fill='%23FFFFFF' stroke='%23C3C8CD' stroke-width='1.62001'/%3E%3Cpath d='M51.0791 36.4983C51.5536 36.0238 51.5536 35.2545 51.0791 34.78L43.3469 27.0478C42.8724 26.5733 42.1031 26.5733 41.6286 27.0478C41.1541 27.5223 41.1541 28.2916 41.6286 28.7661L48.5017 35.6392L41.6286 42.5123C41.1541 42.9868 41.1541 43.7561 41.6286 44.2306C42.1031 44.705 42.8724 44.705 43.3469 44.2306L51.0791 36.4983ZM20.25 36.8542L50.22 36.8542L50.22 34.4242L20.25 34.4242L20.25 36.8542Z' fill='%23C3C8CD'/%3E%3C/svg%3E%0A");

        @include mobile-only {
          width: $icon-width-mobile;
          height: $icon-height-mobile;
        }
      }

      &:hover::before {
        content: url("data:image/svg+xml,%3Csvg viewBox='0 0 70 70' fill='none' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0.810161 34.8382C0.810162 16.0493 16.0414 0.817882 34.8302 0.817883C53.6189 0.817885 68.8502 16.0493 68.8502 34.8382C68.8502 53.6272 53.6189 68.8586 34.8302 68.8586C16.0414 68.8586 0.810159 53.6271 0.810161 34.8382Z' fill='%23F0F1F3' stroke='%23F0F1F3' stroke-width='1.62001'/%3E%3Cpath d='M51.0791 36.5062C51.5536 36.0317 51.5536 35.2625 51.0791 34.788L43.3469 27.0557C42.8724 26.5812 42.1031 26.5812 41.6286 27.0557C41.1541 27.5302 41.1541 28.2995 41.6286 28.774L48.5017 35.6471L41.6286 42.5202C41.1541 42.9947 41.1541 43.764 41.6286 44.2385C42.1031 44.713 42.8724 44.713 43.3469 44.2385L51.0791 36.5062ZM20.25 36.8621L50.22 36.8621L50.22 34.4321L20.25 34.4321L20.25 36.8621Z' fill='%23C3C8CD'/%3E%3C/svg%3E%0A");
      }
    }
  }

  & .carousel .arrow.prev-arrow {
    &::before {
      transform: rotate(180deg);
    }
  }

  & .carousel .carousel__item .carousel__item-inner {
    flex-direction: row;
    align-items: center;
  }
}
```