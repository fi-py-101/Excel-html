2
from openpyxl import load_workbook

# Load the Excel file
file_path = 'your_file.xlsx'
wb = load_workbook(file_path)
ws = wb['Sheet1']  # Replace with your sheet name

# Define the range of cells (e.g., A1:C3)
cell_range = ws['A1:C3']

# Function to convert openpyxl styles to CSS
def get_css_style(cell):
    style = ""
    
    # Font styles
    if hasattr(cell, 'font') and cell.font:
        if cell.font.bold:
            style += "font-weight: bold; "
        if cell.font.italic:
            style += "font-style: italic; "
        if cell.font.underline:
            style += "text-decoration: underline; "
        if hasattr(cell.font, 'color') and cell.font.color:
            if hasattr(cell.font.color, 'rgb'):
                # Extract RGB value and remove the 'FF' prefix (alpha channel)
                rgb_hex = cell.font.color.rgb[2:]  # Skip the first two characters
                style += f"color: #{rgb_hex}; "
    
    # Background color
    if hasattr(cell, 'fill') and cell.fill:
        if hasattr(cell.fill, 'start_color') and cell.fill.start_color:
            if hasattr(cell.fill.start_color, 'rgb'):
                # Extract RGB value and remove the 'FF' prefix (alpha channel)
                rgb_hex = cell.fill.start_color.rgb[2:]  # Skip the first two characters
                style += f"background-color: #{rgb_hex}; "
    
    # Borders
    if hasattr(cell, 'border') and cell.border:
        border_style = ""
        for side in ['left', 'right', 'top', 'bottom']:
            if hasattr(cell.border, side):
                border = getattr(cell.border, side)
                if hasattr(border, 'style') and border.style:
                    border_color = getattr(border, 'color', None)
                    if border_color and hasattr(border_color, 'rgb'):
                        # Extract RGB value and remove the 'FF' prefix (alpha channel)
                        rgb_hex = border_color.rgb[2:]  # Skip the first two characters
                        border_style += f"border-{side}: {border.style} #{rgb_hex}; "
        style += border_style
    
    # Alignment
    if hasattr(cell, 'alignment') and cell.alignment:
        if hasattr(cell.alignment, 'horizontal') and cell.alignment.horizontal:
            style += f"text-align: {cell.alignment.horizontal}; "
        if hasattr(cell.alignment, 'vertical') and cell.alignment.vertical:
            style += f"vertical-align: {cell.alignment.vertical}; "
    
    return style

# Generate HTML table
html_table = '<table style="border-collapse: collapse;">\n'
for row in cell_range:
    html_table += '<tr>\n'
    for cell in row:
        cell_style = get_css_style(cell)
        cell_value = cell.value if cell.value is not None else ""
        html_table += f'<td style="{cell_style}">{cell_value}</td>\n'
    html_table += '</tr>\n'
html_table += '</table>'

# Save the HTML to a file
with open('output.html', 'w') as f:
    f.write(html_table)

print("HTML table generated and saved to output.html")