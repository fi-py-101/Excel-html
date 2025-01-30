from openpyxl import load_workbook
from openpyxl.styles import Font, Fill, Border, Alignment

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
    if cell.font:
        if cell.font.bold:
            style += "font-weight: bold; "
        if cell.font.italic:
            style += "font-style: italic; "
        if cell.font.underline:
            style += "text-decoration: underline; "
        if cell.font.color:
            style += f"color: #{cell.font.color.rgb[2:]}; "  # Convert color to hex
    
    # Background color
    if cell.fill:
        if cell.fill.start_color.rgb:
            style += f"background-color: #{cell.fill.start_color.rgb[2:]}; "
    
    # Borders
    if cell.border:
        border_style = ""
        for side in ['left', 'right', 'top', 'bottom']:
            border = getattr(cell.border, side)
            if border.style:
                border_style += f"border-{side}: {border.style} #{border.color.rgb[2:]}; "
        style += border_style
    
    # Alignment
    if cell.alignment:
        if cell.alignment.horizontal:
            style += f"text-align: {cell.alignment.horizontal}; "
        if cell.alignment.vertical:
            style += f"vertical-align: {cell.alignment.vertical}; "
    
    return style

# Generate HTML table
html_table = '<table style="border-collapse: collapse;">\n'
for row in cell_range:
    html_table += '<tr>\n'
    for cell in row:
        cell_style = get_css_style(cell)
        html_table += f'<td style="{cell_style}">{cell.value}</td>\n'
    html_table += '</tr>\n'
html_table += '</table>'

# Save the HTML to a file
with open('output.html', 'w') as f:
    f.write(html_table)

print("HTML table generated and saved to output.html")