# Alkemio UX 
This document provides guidelines for Alkemio User Experience (UX).

## Alkemio (TM) Brand
The logo for Alkemio is shown below:

<p align="center">
  <a href="http://alkem.io/" target="blank"><img src="https://www.alkemio.foundation/uploads/logos/alkemio-logo.svg" width="200" alt="Alkemio Logo" /></a>
</p>

The logo uses the following color codes:  
| Name | RGB | Usage |  
| - | - |  - |
| Pacific Blue | ![#09BCD4](https://via.placeholder.com/50x20/09BCD4/969696?text=+) `#09BCD4` | Logo
| Neutral light | ![#F9F9F9](https://via.placeholder.com/50x20/F9F9F9/969696?text=+) `#F9F9F9` | Background
| Blue Grey | ![#44546a](https://via.placeholder.com/50x20/44546a/44546a?text=+) `#44546a` | Background
| Off white | ![#e0f5fe](https://via.placeholder.com/50x20/e0f5fe/e0f5fe?text=+) `#e0f5fe` | Background
| Light green | ![#eaf0e0](https://via.placeholder.com/50x20/eaf0e0/eaf0e0?text=+) `#eaf0e0` | Background
| Darkblue | ![#232342](https://via.placeholder.com/50x20/232342/232342?text=+) `#232342` | Text  
| Deep Blue | ![#2D546A](https://via.placeholder.com/50x20/2d546a/2d546a?text=+) `#2D546A` | Text

These colors are used also in certain places in the user interface. However for accessibilty (i.e. contrast) the Pacific Blue color in particular is swapped in text for the Primary color above. 

## User Interface Color Palette
These are the colors currently defined in the web application and their usage:

| Name | Color | Usage |
| -------- | -------- | -------- |
| Primary     | ![#068293](https://via.placeholder.com/50x20/068293/068293?text=+) `#068293`    | Buttons, Large text  |
| Secondary     | ![#00a88f](https://via.placeholder.com/50x20/00a88f/00a88f?text=+) `#00a88f`   | buttons, medium-large text  |
| Positive     | ![#00D4B4](https://via.placeholder.com/50x20/00D4B4/00D4B4?text=+) `#00D4B4`    | Tags  |
| Negative    | ![#D40062](https://via.placeholder.com/50x20/D40062/D40062?text=+) `#D40062`     | Errors  |
| Neutral     | ![#181828](https://via.placeholder.com/50x20/181828/181828?text=+) `#181828`     | Small text and shadows  |
| Neutral Medium    | ![#9e9e9e](https://via.placeholder.com/50x20/9e9e9e/9e9e9e?text=+) `#9e9e9e`     | Tags, Text  |
| Neutral Light     | ![#F9F9F9](https://via.placeholder.com/50x20/F9F9F9/F9F9F9?text=+) `#F9F9F9`     | Background  |
| Text     | ![#181828](https://via.placeholder.com/50x20/181828/181828?text=+) `#181828`    | Text  |
| Background     | ![#FFFFFF](https://via.placeholder.com/50x20/FFFFFF/FFFFFF?text=+) `#FFFFFF`    | Background  |

The above colors are also used with an opacity applied in various locations in the user experience. For example, the divider color is primary with 40% opacity.

## Typography

The typography that is currently defined in the web application

| Name | Family | Size (rem) | Weight | Usage |  
| -------- | -------- | -------- | -------- | -------- |  
| h1 | Montseratt | 2.125 | 600 | TBD |
| h2 | Montseratt | 1.825 | 600 | TBD |
| h3 | Montseratt | 1.625 | 600 | TBD |
| h4 | Montseratt | 1.375 | 600 | TBD |
| h5 | Source Sans Pro | 1.2| 600 | TBD |
| h6 | Source Sans Pro | 1 | 600 | TBD |
| caption | Montseratt | 1 | 600 | tags |  
| button | Montseratt | 1| 600 | TBD |

## General theme definition

- Spacing - defined @ 10px - paddings & margins are derived from it.
- Border Radius - defined @ 5px
- Breakpoints - currently using the bootstrap defaults

| Name | From | To |
| -------- | -------- | -------- |
| XS | 0 | 576 |
| SM | 576 | 768 |
| MD | 768 | 992 |
| lg | 992 | 1200 |
| xl | 1200 | Infinity |

## Components and customizations
The web application uses a component library that is built on top of the [Material UI principles](https://material.io/). The library is https://mui.com/. All components that are used in the application and style overrides need to be listed. Designs should be based on the component library for easier adaptation during the development phase.

### TBD - component definitions