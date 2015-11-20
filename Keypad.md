The `Keypad` class constructs an object that represents a single Keypad attached to the board.

Supported Keypads:

- VKEY
- Waveshare AD (Analog)
- MPR121
- MPR121QR2
- Grove QTouch (AT42QT1070)

## Parameters

- **General Options**
  <span class="abbreviate-table">

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | controller    | string | AT42QT1070, MPR121, MPR121QR2, QTOUCH, VKEY, ANALOG. The Name of the controller to use | ANALOG | Yes       |
  | keys    | array | Mapping of key values |  By Device | No       |
  </span>

- **VKEY Options (`controller: "VKEY"`)** 

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | pin    | number, string | Analog pin | | Yes |

- **ANALOG Options (`controller: "ANALOG"`)** 

  | Property | Type   | Value/Description                       | Default  | Required |
  |---------------|--------|--------------------------------------------|-----------------------------------|----------|
  | pin    | number, string | Analog pin | | Yes |
  | length | number | Number of buttons (required only if `keys` is not specified) | | Yes |

## Shape

## Component Initialization

## API

## Events