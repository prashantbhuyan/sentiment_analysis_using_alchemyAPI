{\rtf1\ansi\ansicpg1252\cocoartf1265\cocoasubrtf210
{\fonttbl\f0\fswiss\fcharset0 Helvetica;\f1\fmodern\fcharset0 Courier;\f2\fmodern\fcharset0 Courier-Oblique;
\f3\fnil\fcharset0 HelveticaNeue;}
{\colortbl;\red255\green255\blue255;\red0\green0\blue109;\red51\green110\blue109;\red245\green245\blue245;
\red38\green38\blue38;\red83\green83\blue83;\red118\green0\blue2;}
\margl1440\margr1440\vieww10800\viewh8400\viewkind0
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural

\f0\fs24 \cf0 Does the old Wall Street adage \'93Good News, Bad Action\'94 and \'93Bad News, Good Action\'94 holds true?\
\
The intuition of the adage is that if the market goes higher despite bad news that the market will continue to go up.  If the market goes down on bad action then it may continue to go down, etc. I have built an application that will help me answer this question.\
\
The goal of this final project was to build a data pipeline between the bloomberg.com website and an analytical engine created in python and a mongo database in order to explore and model the relationship between headline sentiment and market behavior.  The findings could lead to further research in developing an algorithmic trading system.\
\
To View Solution in JSON format\
1) go to https://gist.github.com/prashantbhuyan/639e8168e1963902c674 \
\
To View Solution in NBViewer:\
1) go to http://nbviewer.ipython.org/gist/prashantbhuyan/639e8168e1963902c674\
\
To Run Solution:\
1) Make sure mongo db is running on your machine\
2) Make sure alchemy api is running on your machine\
3) start ipyhon notebook\
4) convert ipynb.json code into .py (import libraries below and specify path)\
\
from runipy.notebook_runner import NotebookRunner\
from IPython.nbformat.current import read\
\
notebook = read(open("fin_sentiment_parser.ipynb.json"), 'json')\
r = NotebookRunner(notebook)\
r.run_notebook()\
\
5) make sure that the number of sentiment scores and the number of market data points are the same because when running the modeling part of the application I need equally sized data sets.  This works out in real life because i can look at more granular market data up to 100 microsecond resolution and the rate at which bloomberg publishes stock headlines will not be any where close to the size of the market data points I\'92ll have.  I do plan on trying to figure out a more elegant solution for this though.\
\
\
\
Obtain, Scrub and Store Web Data:\
\
1) The application scrapes the bloomberg.com website for relevant stock market data using python and beautiful soup and stores that data to a document based mongo database (so that i can build that database over time since the data is stored in an extensible manner),\
\
2) Sends data queried from the database (stock market headlines) to the alchemy api and receives sentiment information about each headline.\
\
3) Parses the response from alchemy api and transforms the data into numerical values and stores the data into data structures that can be analyzed and manipulated in numpy and pandas. \
\
4) Using pandas the application pulls stock market data from yahoo finance and stores the data into a data frame.\
\
Explore Data:\
\
5) The distribution of the sentiment scores is analyzed descriptively using numpy and visuals are plotted using matplotlib and pylab. \
\
6) The distribution of market returns (cumulative and daily adjusted close prices) are analyzed descriptively using numpy and visuals are plotted using matplotlib and pylab.\
\
Model Data:\
\
7) Correlation Analysis between cumulative adjusted returns of the market and the sentiment scores show a correlation of:\
\
\pard\pardeftab720\qr

\f1\fs28 \cf2 In\'a0[68]:\
\pard\pardeftab720

\f2\i \cf3 \cb4 # model the data - correlation analysis of cumulative market returns and sentiment scores
\f1\i0 \cf5 \

\f2\i \cf3 # function returns (Pearson\'92s correlation coefficient, 2-tailed p-value)
\f1\i0 \cf5 \
stats\cf6 .\cf5 pearsonr(cum_rets[\cf6 3\cf5 :\cf6 28\cf5 ], ranked_sentiment_scores)\
\pard\pardeftab720\qr
\cf7 \cb1 Out[68]:\
\pard\pardeftab720
\cf0 (-0.39979160333165109, 0.047694579773680371)
\f0\fs24 \
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural
\cf0 \
8) A Regression Analysis between cumulative adjusted returns of the market and the sentiment scores yield the following results:\
\pard\pardeftab720\qr

\f1\fs28 \cf2 In\'a0[80]:\
\pard\pardeftab720

\f2\i \cf3 \cb4 # ordinary least squares regression
\f1\i0 \cf5 \
model \cf6 =\cf5  smf\cf6 .\cf5 OLS(cum_rets[\cf6 3\cf5 :\cf6 28\cf5 ],ranked_sentiment_scores)\
results \cf6 =\cf5  model\cf6 .\cf5 fit()\
results\cf6 .\cf5 summary()\
\pard\pardeftab720\qr
\cf7 \cb1 Out[80]:\
\pard\pardeftab720\qc

\f3 \cf0 OLS Regression Results\

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrt\brdrs\brdrw20\brdrcf0 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Dep. Variable:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 Adj Close\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 R-squared:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 0.265\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Model:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 OLS\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 Adj. R-squared:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 0.234\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Method:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 Least Squares\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 F-statistic:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 8.636\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Date:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 Sun, 21 Dec 2014\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 Prob (F-statistic):\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 0.00718\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Time:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 23:27:24\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 Log-Likelihood:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 74.690\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 No. Observations:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 25\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 AIC:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 -147.4\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Df Residuals:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 24\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 BIC:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 -146.2\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrb\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2400\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth2240\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2300\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth1020\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Df Model:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 1\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 \cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 \cell \lastrow\row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \tamart280 \trbrdrt\brdrs\brdrw20\brdrcf0 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth320\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx1440
\clvertalc \clshdrawnil \clwWidth980\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2880
\clvertalc \clshdrawnil \clwWidth880\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth820\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx5760
\clvertalc \clshdrawnil \clwWidth720\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx7200
\clvertalc \clshdrawnil \clwWidth2320\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720
\cf0 \cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 coef\cell 
\pard\intbl\itap1\pardeftab720
\cf0 std err\cell 
\pard\intbl\itap1\pardeftab720
\cf0 t\cell 
\pard\intbl\itap1\pardeftab720
\cf0 P>|t| \cell 
\pard\intbl\itap1\pardeftab720
\cf0 [95.0% Conf. Int.]\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \tamart280 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrb\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth320\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx1440
\clvertalc \clshdrawnil \clwWidth980\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2880
\clvertalc \clshdrawnil \clwWidth880\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth820\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx5760
\clvertalc \clshdrawnil \clwWidth720\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx7200
\clvertalc \clshdrawnil \clwWidth2320\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720
\cf0 x1\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 -0.0162\cell 
\pard\intbl\itap1\pardeftab720
\cf0 0.006\cell 
\pard\intbl\itap1\pardeftab720
\cf0 -2.939\cell 
\pard\intbl\itap1\pardeftab720
\cf0 0.007\cell 
\pard\intbl\itap1\pardeftab720
\cf0 -0.028 -0.005\cell \lastrow\row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \tamart280 \trbrdrt\brdrs\brdrw20\brdrcf0 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2080\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth820\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2340\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth860\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Omnibus:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 7.224\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 Durbin-Watson:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 0.550\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \tamart280 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2080\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth820\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2340\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth860\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Prob(Omnibus):\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 0.027\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 Jarque-Bera (JB):\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 5.569\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \tamart280 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2080\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth820\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2340\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth860\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Skew:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 -1.130\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 Prob(JB):\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 0.0618\cell \row

\itap1\trowd \taflags1 \trgaph108\trleft-108 \tamart280 \trbrdrl\brdrs\brdrw20\brdrcf0 \trbrdrb\brdrs\brdrw20\brdrcf0 \trbrdrr\brdrs\brdrw20\brdrcf0 
\clvertalc \clshdrawnil \clwWidth2080\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx2160
\clvertalc \clshdrawnil \clwWidth820\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx4320
\clvertalc \clshdrawnil \clwWidth2340\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx6480
\clvertalc \clshdrawnil \clwWidth860\clftsWidth3 \clmart280 \clmarl560 \clmarb280 \clmarr560 \clbrdrt\brdrs\brdrw20\brdrcf0 \clbrdrl\brdrs\brdrw20\brdrcf0 \clbrdrb\brdrs\brdrw20\brdrcf0 \clbrdrr\brdrs\brdrw20\brdrcf0 \clpadt80 \clpadl80 \clpadb80 \clpadr80 \gaph\cellx8640
\pard\intbl\itap1\pardeftab720

\b \cf0 Kurtosis:\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 3.486\cell 
\pard\intbl\itap1\pardeftab720

\b \cf0 Cond. No.\cell 
\pard\intbl\itap1\pardeftab720

\b0 \cf0 1.00\cell \lastrow\row
\pard\tx720\tx1440\tx2160\tx2880\tx3600\tx4320\tx5040\tx5760\tx6480\tx7200\tx7920\tx8640\pardirnatural

\f0\fs24 \cf0 \
\
Interpret Results:\
\
Is there a relationship between market returns and headline sentiment?  \
\
The primary reason to create a database pipeline was to collect more headline data from bloomberg.  The reason I chose bloomberg is that they seem to have an inside track as a curator for relevant market moving information- perhaps that\'92s due to their extensive network of reporters (as opposed to twitter where you have a lot of noise- I tried to use twitter to build a real time feed but I had limited transactions per day on the alchemy api trial - but in the future i may purchase the api license and build a pipeline to the twitter firehose and use something like bloomberg as a layer that helps me to filter less relevant data).   \
\
The amount of sentiment data that I used on this analysis was very small-but I will re-run the application every day and build the database and see if my results hold up over time.\
\
I found in these data that I studied a surprising negative correlation between sentiment scores and market behavior as proxied by the SPY Cash Index.  In this sampling I found sentiment to be fairly negative but market returns to be positive.  \
\
I found an adjusted r-squared of 23.4 with a p value of .007 which suggests that in fact market returns are a significant predictor of headline sentiment which is interesting because market returns are going higher but headline sentiment over my sample was negative- if my analytics hold over larger datasets then this reading for example strong returns and negative headlines may yield a buy signal.\
\
\
}