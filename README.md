3
from openpyxl import load_workbook

def get_rgb_hex(color):
    """Safely extract RGB hex from openpyxl color objects."""
    if color is None:
        return None
    if hasattr(color, 'rgb') and color.rgb is not None:
        # Remove alpha channel (first two characters) if present
        return color.rgb[2:] if len(color.rgb) == 8 else color.rgb
    elif hasattr(color, 'indexed') and color.indexed is not None:
        # Handle indexed colors (fallback to black)
        return '000000'  # Default to black
    return None  # Unsupported color type

def get_css_style(cell):
    style = []
    
    # Font styles
    if cell.font:
        font = cell.font
        if font.bold:
            style.append("font-weight: bold")
        if font.italic:
            style.append("font-style: italic")
        if font.underline:
            style.append("text-decoration: underline")
        if font.color:
            rgb_hex = get_rgb_hex(font.color)
            if rgb_hex:
                style.append(f"color: #{rgb_hex}")

    # Background color
    if cell.fill:
        fill = cell.fill
        if fill.start_color:
            rgb_hex = get_rgb_hex(fill.start_color)
            if rgb_hex:
                style.append(f"background-color: #{rgb_hex}")

    # Borders
    if cell.border:
        border = cell.border
        for side in ['left', 'right', 'top', 'bottom']:
            border_side = getattr(border, side)
            if border_side.style:
                border_color = getattr(border_side, 'color', None)
                rgb_hex = get_rgb_hex(border_color)
                if rgb_hex:
                    style.append(f"border-{side}: 1px solid #{rgb_hex}")

    # Alignment
    if cell.alignment:
        align = cell.alignment
        if align.horizontal:
            style.append(f"text-align: {align.horizontal}")
        if align.vertical:
            style.append(f"vertical-align: {align.vertical}")

    return "; ".join(style)

# Load the Excel file
wb = load_workbook('your_file.xlsx')
ws = wb.active  # Use the active sheet

# Generate HTML table
html = ['<table style="border-collapse: collapse; border: 1px solid black;">']
for row in ws.iter_rows(min_row=1, max_row=3, min_col=1, max_col=3):  # A1:C3
    html.append('<tr>')
    for cell in row:
        cell_value = cell.value if cell.value is not None else ""
        cell_style = get_css_style(cell)
        html.append(f'<td style="{cell_style}">{cell_value}</td>')
    html.append('</tr>')
html.append('</table>')

# Save to file
with open('output.html', 'w') as f:
    f.write("\n".join(html))