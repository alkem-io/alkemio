# Alkemio UX 
This document provides guidelines for Alkemio User Experience (UX).

## Alkemio (TM) Brand
The logo for Alkemio is shown below:

<p align="center">
  <a href="http://alkem.io/" target="blank"><img src="https://alkem.io/uploads/logos/alkemio-logo.svg" width="200" alt="Alkemio Logo" /></a>
</p>

The logo uses the following two color codes:
| Name | Color | Usage |
| -------- | -------- | -------- |
| Pacific Blue     | ![#09bcd4](https://via.placeholder.com/15/09bcd4/000000?text=+) `#09bcd4`    | icon   |
| Deep Blue     | ![#2d546a](https://via.placeholder.com/15/2d546a/000000?text=+) `#2d546a`     | text  | 

These colors are used also in certain places in the user interface. However for accessibilty (i.e. contrast) the Pacific Blue color in particular is swapped in text for the Primary color above. 

## User Interface Color Palette
These are the colors currently defined in the web application and their usage:

| Name | Color | Usage |
| -------- | -------- | -------- |
| Primary     | ![#068293](https://via.placeholder.com/15/068293/000000?text=+) `#068293`    | buttons, large text  |
| Secondary     | TBD    | buttons, medium-large text  |
| Positive     | ![#00A88F](https://via.placeholder.com/15/00A88F/000000?text=+) `#00A88F`    | tags  |
| Warning    | TBD     | warnings  |
| Negative    | ![#D40062](https://via.placeholder.com/15/D40062/000000?text=+) `#D40062`     | errors  |
| Neutral     | ![#181828](https://via.placeholder.com/15/181828/000000?text=+) `#181828`     | small text and shadows  |
| Neutral plus    | ![#B8BAC8](https://via.placeholder.com/15/B8BAC8/000000?text=+) `#B8BAC8`     | tags, text  |
| Neutral light     | ![#F9F9F9](https://via.placeholder.com/15/F9F9F9/000000?text=+) `#F9F9F9`     | backgrounds  |
| Background     | ![#FFFFFF](https://via.placeholder.com/15/FFFFFF/000000?text=+) `#FFFFFF`    | backgrounds  |

The above colors are also used with an opacity applied in various locations in the user experience. For example, the divider color is primary with 40% opacity.

## Typography

The typography that is currently defined in the web application

| Name | Family | Size | Weight | Usage |
| -------- | -------- | -------- | -------- | -------- |
| h1 | monseratt | 48 | Regular | TBD |
| h2 | sourceSansPro (current monseratt) | 36 | Bold (Current regular) | TBD |
| h3 | sourceSansPro | 24 | Bold (Current regular) | TBD |
| h4 | sourceSansPro (current monseratt) | 20 (Current 22) | Bold (Current regular) | TBD |
| h5 | sourceSansPro | 18 | Regular | TBD |
| caption | monseratt | 12 | Regular | tags |
| body1 | sourceSansPro | 16 | Regular | TBD |
| body2 | sourceSansPro | 14 | Regular | TBD |
| button | monseratt | 14 | buttons |

## General theme definition

- Spacing - defined @ 10px - paddings & margins are derived from it.
- Border Radius - defined @ 5px
- Breakpoints - currently using the bootstrap defaults

| Name | From | To |
| -------- | -------- | -------- |
| XS | 0 | 576 |
| SM | 576 | 768 |
| MD | 768 | 992 |
| MD | 992 | 1200 |

## Components and customizations
The web application uses a component library that is built on top of the [Material UI principles](https://material.io/). The library is https://mui.com/. All components that are used in the application and style overrides need to be listed. Designs should be based on the component library for easier adaptation during the development phase.

### TBD - component definitions