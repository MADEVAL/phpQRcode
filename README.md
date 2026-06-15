# globus-studio/phpqrcode

Zero-dependency QR Code generator for PHP 7.4–9.0.

[![CI](https://github.com/MADEVAL/phpQRcode/actions/workflows/ci.yml/badge.svg)](https://github.com/MADEVAL/phpQRcode/actions)
[![PHP](https://img.shields.io/badge/php-7.4%20|%208.x%20|%209.0-8892BF)](https://github.com/MADEVAL/phpQRcode)
[![License](https://img.shields.io/github/license/MADEVAL/phpQRcode)](LICENSE)

## Features

- Dead simple API — one line to generate QR
- Zero runtime dependencies
- PHP 7.4 / 8.0 / 8.1 / 8.2 / 8.3 / 8.4 / 8.5 / 9.0
- QR Code versions 1–40, all error correction levels
- Multiple output formats: SVG, PNG, HTML, ASCII, raw matrix
- ~100% test coverage (186 tests, 2209 assertions)
- PHPStan level max, zero errors
- 17x faster with fixed mask pattern
- PSR-4 autoloading

## Requirements

- PHP >= 7.4
- ext-gd (optional, only for PNG rendering)

## Installation

```bash
composer require globus-studio/phpqrcode
```

## Usage

```php
use GlobusStudio\QRCode\QRCode;

// SVG string
$svg = QRCode::svg('https://example.com');

// PNG as data URI (requires ext-gd)
$png = QRCode::png('https://example.com');

// HTML table
$html = QRCode::html('https://example.com');

// ASCII art
echo QRCode::string('https://example.com');

// Raw boolean matrix
$matrix = QRCode::matrix('https://example.com');
```

## Options

```php
use GlobusStudio\QRCode\QRCode;
use GlobusStudio\QRCode\ErrorCorrection\ErrorCorrectionLevel;

$svg = QRCode::svg('data', [
    'level' => ErrorCorrectionLevel::H,
    'size' => 4,
    'margin' => 2,
    'mask' => 3,            // fixed mask 0-7 (skips auto-selection, 17x faster)
    'foreground' => '#000000',
    'background' => '#ffffff',
]);
```

## Performance

By default the encoder tests all 8 mask patterns to find the optimal one. For batch generation or when speed matters, pass a fixed `mask` (0–7):

```php
// ~180ms per QR (auto mask selection)
$svg = QRCode::svg($data);

// ~11ms per QR (fixed mask, 17x faster)
$svg = QRCode::svg($data, ['mask' => 0]);
```

## Object API

```php
use GlobusStudio\QRCode\QRCode;
use GlobusStudio\QRCode\ErrorCorrection\ErrorCorrectionLevel;
use GlobusStudio\QRCode\Renderer\SvgRenderer;

$qr = new QRCode('https://example.com', ErrorCorrectionLevel::H);
$svg = $qr->render(new SvgRenderer(['size' => 6]));
$matrix = $qr->getMatrix();
```

## Error Correction Levels

| Level | Recovery |
|-------|----------|
| L | ~7% |
| M | ~15% |
| Q | ~25% |
| H | ~30% |

## Renderers

| Renderer | Dependencies | Output |
|----------|-------------|--------|
| SvgRenderer | none | SVG XML string |
| HtmlRenderer | none | HTML table |
| StringRenderer | none | ASCII/UTF-8 |
| PngRenderer | ext-gd | PNG binary |
| RawRenderer | none | JSON / int[][] |

## Credits

Inspired by the original [QRCode for PHP](http://www.d-project.com/) by **Kazuhiko Arase** (2009).
Rewritten from scratch with modern PHP practices, strict typing, full test coverage, and zero deprecations.

## License

MIT — see [LICENSE](LICENSE).
