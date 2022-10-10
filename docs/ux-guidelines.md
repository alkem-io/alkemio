# Alkemio UX 
This document provides guidelines for Alkemio User Experience (UX).

## Alkemio (TM) Brand
The logo for Alkemio is shown below:

<p align="center">
  <a href="https://www.alkemio.foundation" target="blank"><img src="https://www.alkemio.foundation/uploads/logos/alkemio-logo.svg" width="200" alt="Alkemio Logo" /></a>
</p>

The logo uses the following color codes:  
| Name | RGB | Usage |    
| - | - |  - |  
| Deep Blue | ![](https://placehold.jp/2D546A/ffffff/50x20.png?text=%20) `#2D546A` | Logo text  
| Pacific Blue | ![](https://placehold.jp/09BCD4/ffffff/50x20.png?text=%20) `#09BCD4` | Logo 
| Neutral light | ![#F9F9F9](https://placehold.jp/F9F9F9/ffffff/50x20.png?text=%20) `#F9F9F9` | Background  
| Blue Grey | ![#44546a](https://placehold.jp/44546a/ffffff/50x20.png?text=%20) `#44546a` | Background  
| Off white | ![#e0f5fe](https://placehold.jp/e0f5fe/ffffff/50x20.png?text=%20) `#e0f5fe` | Background  
| Light green | ![#eaf0e0](https://placehold.jp/eaf0e0/ffffff/50x20.png?text=%20) `#eaf0e0` | Background  
| Darkblue | ![#181828](https://placehold.jp/181828/ffffff/50x20.png?text=%20) `#181828` | Text    
  
These colors are used also in certain places in the user interface. However for accessibilty (i.e. contrast) the Pacific Blue color in particular is swapped in text for the Primary color above.   
  
## User Interface Color Palette  
These are the colors currently defined in the web application and their usage:  
  
| Name | Color | Usage |  
| -------- | -------- | -------- |  
| Primary     | ![#068293](https://placehold.jp/068293/ffffff/50x20.png?text=%20) `#068293`    | Buttons, Large text  |  
| Secondary     | ![#00a88f](https://placehold.jp/00a88f/ffffff/50x20.png?text=%20) `#00a88f`   | buttons, medium-large text  |  
| Positive     | ![#00D4B4](https://placehold.jp/00D4B4/ffffff/50x20.png?text=%20) `#00D4B4`    | Tags  |  
| Negative    | ![#D40062](https://placehold.jp/D40062/ffffff/50x20.png?text=%20) `#D40062`     | Errors  |  
| Neutral     | ![#181828](https://placehold.jp/181828/ffffff/50x20.png?text=%20) `#181828`     | Small text and shadows  |  
| Neutral Medium    | ![#9e9e9e](https://placehold.jp/9e9e9e/ffffff/50x20.png?text=%20) `#9e9e9e`     | Tags, Text  |  
| Neutral Light     | ![#F9F9F9](https://placehold.jp/F9F9F9/ffffff/50x20.png?text=%20) `#F9F9F9`     | Background  |  
| Text     | ![#181828](https://placehold.jp/181828/ffffff/50x20.png?text=%20) `#181828`    | Text  |  
| Background     | ![#FFFFFF](https://placehold.jp/FFFFFF/ffffff/50x20.png?text=%20) `#FFFFFF`    | Background  |

The above colors are also used with an opacity applied in various locations in the user experience. For example, the divider color is primary with 40% opacity.

# New User Interface Color Palette
These are the colours that will soon be defined in the web application: 

| Name | Color | Usage |  
| -------- | -------- | -------- |  
| Primary     | ![#065F6B](https://placehold.jp/065F6B/ffffff/50x20.png?text=%20) `#065F6B`    | Challenges, buttons, large text  |  
| Secondary     | ![#1D384A](https://placehold.jp/1D384A/ffffff/50x20.png?text=%20) `#1D384A`   | Hubs, dark text for Opportunities  |  
| Positive     | ![#A2D2DB](https://placehold.jp/A2D2DB/ffffff/50x20.png?text=%20) `#A2D2DB`    | Opportunities, for text only to be used in combination with 'Secondary', not with white  |  
| Light    | ![#DEEFF6](https://placehold.jp/DEEFF6/ffffff/50x20.png?text=%20) `#DEEFF6`     | Background  |  
| Negative    | ![#B30000](https://placehold.jp/B30000/ffffff/50x20.png?text=%20) `#B30000`     | Errors  |  
| Neutral     | ![#181828](https://placehold.jp/181828/ffffff/50x20.png?text=%20) `#181828`     | Small text and shadows  |  
| Neutral Medium    | ![#9e9e9e](https://placehold.jp/9e9e9e/ffffff/50x20.png?text=%20) `#9e9e9e`     | Tags, Text  |  
| Neutral Light     | ![#F9F9F9](https://placehold.jp/F9F9F9/ffffff/50x20.png?text=%20) `#F9F9F9`     | Background  |  
| Text     | ![#181828](https://placehold.jp/181828/ffffff/50x20.png?text=%20) `#181828`    | Text  |  
| Background     | ![#FFFFFF](https://placehold.jp/FFFFFF/ffffff/50x20.png?text=%20) `#FFFFFF`    | Background  |


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
