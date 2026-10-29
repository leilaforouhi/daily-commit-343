
import argparse
from pathlib import Path

import qrcode


def generate_qr_code(content, output_file="qrcode.png"):
    if not content.strip():
        raise ValueError("Content cannot be empty")

    output_path = Path(output_file)

    if output_path.suffix.lower() != ".png":
        raise ValueError("Output file must have a .png extension")

    image = qrcode.make(content)
    image.save(output_path)

    return output_path


if __name__ == "__main__":
    parser = argparse.ArgumentParser(
        description="Generate a QR code from text or a URL."
    )

    parser.add_argument(
        "content",
        help="Text or URL to encode",
    )

    parser.add_argument(
        "-o",
        "--output",
        default="qrcode.png",
        help="Output PNG filename",
    )

    args = parser.parse_args()

    try:
        output = generate_qr_code(args.content, args.output)
        print(f"QR code saved successfully: {output.resolve()}")

    except (ValueError, OSError) as error:
        print(f"Error: {error}")
