# email-migration

# script to convert copy paste from godaddy web site
# GoDaddy doesnt let you download the list of your
# emaila accounts so you need to manually copy paste
# the info into a text file and parse it out, they
# also make the URL hard to find.
#
# https://productivity.godaddy.com/#/
#
# How to Use, just copy in your browser the account list
# and paste into text file in this format;

# <Name of the account>
# <Email>
#
# Upgrade
#
# Manage

# Example:
# -- Start text file --
# John Doe
# john.doe@url.com
#
# Upgrade
#
# Manage
# Jane Sue
# jane.sue@url.coma
#
# Upgrade
#
# Manage
# -- End text file --

# This turns lines into columns, a line with 'Manage'
# signifies a next row.

import csv

input_file = "list.txt"     # Pasted output file copied from GoDaddy
output_file = "output.csv"

rows = []
current_row = []

with open(input_file, "r", encoding="utf-8") as file:
    for line in file:
        line = line.strip()
        if line.lower() == "manage":
            if current_row:
                rows.append(current_row)
                current_row = []
        elif line:  # skip empty lines
            current_row.append(line)

# In case the last entry doesn't end with "Manage"
if current_row:
    rows.append(current_row)

# Pad rows to match column count (4)
for row in rows:
    while len(row) < 4:
        row.append("")

# Write CSV with proper line endings
with open(output_file, "w", encoding="utf-8", newline="") as csvfile:
    writer = csv.writer(csvfile)
    writer.writerow(["Name", "Role", "Email", "Status"])  # optional headers
    writer.writerows(rows)

print(f"Done! {len(rows)} rows written to {output_file}")
