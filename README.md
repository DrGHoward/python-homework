# python-homework

from pathlib import Path
import csv
csvpath = Path('Resources','budget_data.csv')                                                
output = Path('Resources','results.txt')

total_months = 0
month_of_change = []
net_change_list = []
greatest_increase = ["", 0]
greatest_decrease = ["", 9999999999999999999]
total_net = 0

with open(csvpath) as financial_data:
    reader = csv.reader(financial_data)

    # Read the header row
    header = next(reader)

    
    first_row = next(reader)
    total_months = total_months + 1
    total_net = total_net + int(first_row[1])
    prev_net = int(first_row[1])

    for row in reader:

        #  total
        total_months = total_months + 1
        total_net = total_net + int(row[1])

        # net change
        net_change = int(row[1]) - prev_net
        prev_net = int(row[1])
        net_change_list = net_change_list + [net_change]
        month_of_change = month_of_change + [row[0]]

        # greatest increase
        if net_change > greatest_increase[1]:
            greatest_increase[0] = row[0]
            greatest_increase[1] = net_change

        # greatest decrease
        if net_change < greatest_decrease[1]:
            greatest_decrease[0] = row[0]
            greatest_decrease[1] = net_change

net_monthly_avg = round(sum(net_change_list) / len(net_change_list),2)


with open(output, "w") as txt_file:
    txt_file.write(f"Financial Analysis\n")
    txt_file.write(f"----------------------------\n")
    txt_file.write(f"Total Months: {total_months}\n")
    txt_file.write(f"Total: ${total_net}\n")
    txt_file.write(f"Average  Change: ${net_monthly_avg}\n")
    txt_file.write(f"Greatest Increase in Profits: {greatest_increase[0]} (${greatest_increase[1]})\n")
    txt_file.write(f"Greatest Decrease in Profits: {greatest_decrease[0]} (${greatest_decrease[1]})\n")
