import twint
from datetime import datetime
from io import StringIO 
import sys
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

class Capturing(list):
    def __enter__(self):
        self._stdout = sys.stdout
        sys.stdout = self._stringio = StringIO()
        return self
    def __exit__(self, *args):
        self.extend(self._stringio.getvalue().splitlines())
        del self._stringio    # free up some memory
        sys.stdout = self._stdout
c = twint.Config()

#gathers and formats input
username=input("Enter Twitter Name: ")

if username[0]=='@':
    username=username[1:]
c.Username = username

c.Format = "{date} {time} {timezone}"
c.Limit = 300
c.Store_csv = False
c.Output = "none"
with Capturing() as output:
 twint.run.Search(c)

#sets x axis times
xAxis=['12 AM','01 AM','02 AM','03 AM','04 AM','05 AM','06 AM','07 AM','08 AM','09 AM','10 AM','11 AM','12 PM','01 PM','02 PM','03 PM','04 PM','05 PM','06 PM','07 PM','08 PM','09 PM','10 PM','11 PM']


yAxis=[]
#prepares y axis by sorting times and creating list to be converted to AM/PM later
for i in output:
    try:
        time=datetime.strptime(i, '%Y-%m-%d %H:%M:%S %z')
    except:
        break
    time=time.strftime('%H')
    yAxis.append(int(time))
yAxis.sort()
yAxis2=[]
#converts y axis to AM/PM after sorting
for i in yAxis:
    time=datetime.strptime(str(i), '%H')
    time=time.strftime('%I %p')
    yAxis2.append(time)
yAxis2=np.array(yAxis2)
count=0
yAxis3=[]

#counts number of time occurrences per AM/PM hour 
for i in range(0,24):
    yAxis3.append(yAxis.count(i))

#creates bar chart of occurrences
s=pd.DataFrame({'lab':xAxis, 'Occurrences':yAxis3})
grammar="'"
if not(c.Username[-1]=='s' or c.Username[-1]=='S'):
   grammar="'s"
ax=s.plot.bar('lab','Occurrences',title =c.Username+grammar+ " # of Tweets on Each Hour",figsize=(15,10),legend=True, fontsize=12)
ax.set_xlabel("Hours",fontsize=12)
ax.set_ylabel("Occurrences",fontsize=12)
plt.show()
