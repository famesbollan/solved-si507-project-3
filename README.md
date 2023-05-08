Download Link: https://assignmentchef.com/product/solved-si507-project-3
<br>
<h1>Project Overview</h1>

Learning Goals

<ul>

 <li>Be able to import data from CSV files and JSON into a relational database</li>

 <li>Be able to write a range of SQL queries to extract data from a relational database</li>

 <li>Gain experience writing interactive command line programs that support a range of commands and options</li>

</ul>




You will write a program that creates a database to store information about gourmet chocolate bars. This data was originally retrieved from Kaggle (<a href="https://www.kaggle.com/rtatman/chocolate-bar-ratings/data">https://www.kaggle.com/rtatman/chocolate-bar-ratings/data</a>), but you will be working with a cleaned-up version of the data. You will also be working with JSON data that was retrieved from <a href="https://restcountries.eu/">https://restcountries.eu/</a>. Both data files are provided to you in the starter files directory.




After loading the data into the database, you will add the ability for a user to issue several different types of queries to extract information from the database.




To get started, get the data sources and the starter files from <a href="https://www.dropbox.com/sh/nqlt0lrq3qvi1bn/AAAYzztncljFrOWQHfHaXwtua?dl=0">this Dropbox folder</a>.




When you are done, turn in your version of the proj3_choc.py file to Canvas.

<h2>Part 1: Populating the Database (60 points)</h2>

For part 1 you need to create a new database and add data to it from the files flavors_of_cacao_cleaned.csv and countries.json that are included in the starter repo. To work with the unit tests we have provided, you will need to use the following table and column names.




<table width="624">

 <tbody>

  <tr>

   <td width="185">Table: Bars</td>

   <td width="113"> </td>

   <td width="251"> </td>

  </tr>

  <tr>

   <td width="185">Id (primary key)</td>

   <td width="113">Integer</td>

   <td width="251">Primary key, assigned by DB</td>

  </tr>

  <tr>

   <td width="185">Company</td>

   <td width="113">Text</td>

   <td width="251">Name of the company who makes the bar</td>

  </tr>

  <tr>

   <td width="185">SpecificBeanBarName</td>

   <td width="113">Text</td>

   <td width="251">The name of the bar itself, or sometimes the name of the bean</td>

  </tr>

  <tr>

   <td width="185">REF</td>

   <td width="113">Text</td>

   <td width="251">Dunno what this is.</td>

  </tr>

  <tr>

   <td width="185">ReviewDate</td>

   <td width="113">Text</td>

   <td width="251">Date review was done</td>

  </tr>

  <tr>

   <td width="185">CocoaPercent</td>

   <td width="113">Real</td>

   <td width="251">% of cocoa in the bar</td>

  </tr>

  <tr>

   <td width="185">CompanyLocationId</td>

   <td width="113">Integer</td>

   <td width="251">Foreign key – points to Countries</td>

  </tr>

  <tr>

   <td width="185">Rating</td>

   <td width="113">Real</td>

   <td width="251">Rating given by chocolate experts</td>

  </tr>

  <tr>

   <td width="185">BeanType</td>

   <td width="113">Text</td>

   <td width="251">Category of the cocoa bean</td>

  </tr>

  <tr>

   <td width="185">BroadBeanOriginId</td>

   <td width="113">Integer</td>

   <td width="251">Foreign key – points to Countries</td>

  </tr>

 </tbody>

</table>







<table width="624">

 <tbody>

  <tr>

   <td width="183">Table: Countries</td>

   <td width="115"> </td>

   <td width="251"> </td>

  </tr>

  <tr>

   <td width="183">Id (primary key)</td>

   <td width="115">Integer</td>

   <td width="251">Primary key, assigned by DB</td>

  </tr>

  <tr>

   <td width="183">Alpha2</td>

   <td width="115">Text</td>

   <td width="251">2 letter country code</td>

  </tr>

  <tr>

   <td width="183">Alpha3</td>

   <td width="115">Text</td>

   <td width="251">3 letter country code</td>

  </tr>

  <tr>

   <td width="183">EnglishName</td>

   <td width="115">Text</td>

   <td width="251">English name for country</td>

  </tr>

  <tr>

   <td width="183">Region</td>

   <td width="115">Text</td>

   <td width="251">Broad region where country is located.</td>

  </tr>

  <tr>

   <td width="183">Subregion</td>

   <td width="115">Text</td>

   <td width="251">More specific subregion where country is located.</td>

  </tr>

  <tr>

   <td width="183">Population</td>

   <td width="115">Integer</td>

   <td width="251">Country’s population</td>

  </tr>

  <tr>

   <td width="183">Area</td>

   <td width="115">Real</td>

   <td width="251">Country’s area in km<sup>2</sup></td>

  </tr>

 </tbody>

</table>




Note that the Bars table references the Countries table twice–with two Foreign Keys. You will need to make sure that all of the relations are correctly inserted into your database.




<strong>Grading (all points include passing relevant tests):</strong>

<ul>

 <li><strong>[20 points] Read all data from CSV into Bars table</strong></li>

 <li><strong>[20 points] Read all data from JSON into Countries table</strong></li>

 <li><strong>[20 points] Insert correct keys to model relationships</strong></li>

</ul>

<h2>Part 2: Implement Query Interface (100 points)</h2>

To prepare for supporting interactive queries, in part 2 you will implement a function “process_command” that takes a command string and returns a list of tuples representing records that match the query.




Your process_command function must be able to support four main commands, along with a variety of parameters for each. The four commands are ‘bars’, ‘companies’, ‘countries’, and ‘regions.’ Each command supports parameters and provides results as detailed below.




<ul>

 <li>bars

  <ul>

   <li>Description: Lists chocolate bars, according the specified parameters.</li>

   <li>Parameters:

    <ul>

     <li>sellcountry=&lt;alpha2&gt; | sourcecountry=&lt;alpha2&gt; | sellregion=&lt;name&gt; | sourceregion=&lt;name&gt; [default: none]

      <ul>

       <li>Description: Specifies a country or region within which to limit the results, and also specifies whether to limit by the seller (or manufacturer) or by the bean origin source.</li>

      </ul></li>

     <li>ratings | cocoa [default: ratings]

      <ul>

       <li>Description: Specifies whether to sort by rating or cocoa percentage</li>

      </ul></li>

     <li>top=&lt;limit&gt; | bottom=&lt;limit&gt; [default: top=10]

      <ul>

       <li>Description: Specifies whether to list the top &lt;limit&gt; matches or the bottom &lt;limit&gt; matches.</li>

      </ul></li>

     <li>companies

      <ul>

       <li>Description: Lists chocolate bars sellers according to the specified parameters. Only companies that <strong>sell more than 4</strong> kinds of bars are listed in results.</li>

       <li>Parameters:

        <ul>

         <li>country=&lt;alpha2&gt; | region=&lt;name&gt; [default: none]

          <ul>

           <li>Description: Specifies a country or region within which to limit the results.</li>

          </ul></li>

         <li>ratings | cocoa | bars_sold [default: ratings]

          <ul>

           <li>Description: Specifies whether to sort by rating, cocoa percentage, or the number of different types of bars sold</li>

          </ul></li>

         <li>top=&lt;limit&gt; | bottom=&lt;limit&gt; [default: top=10]

          <ul>

           <li>Description: Specifies whether to list the top &lt;limit&gt; matches or the bottom &lt;limit&gt; matches.</li>

          </ul></li>

         <li>countries

          <ul>

           <li>Description: Lists countries according to specified parameters. Only countries that sell/source <strong>more than 4</strong> kinds of bars are listed in results.</li>

           <li>Parameters:

            <ul>

             <li>region=&lt;name&gt; [default: none]

              <ul>

               <li>Description: Specifies a region within which to limit the results.</li>

              </ul></li>

             <li>sellers | sources [default: sellers]

              <ul>

               <li>Description: Specifies whether to select countries based sellers or bean sources.</li>

              </ul></li>

             <li>ratings | cocoa | bars_sold [default: ratings]

              <ul>

               <li>Description: Specifies whether to sort by rating, cocoa percentage, or the number of different types of bars sold</li>

              </ul></li>

             <li>top=&lt;limit&gt; | bottom=&lt;limit&gt; [default: top=10]

              <ul>

               <li>Description: Specifies whether to list the top &lt;limit&gt; matches or the bottom &lt;limit&gt; matches.</li>

              </ul></li>

             <li>regions

              <ul>

               <li>Description: Lists regions according to specified parameters. Only regions that sell/source <strong>more than 4</strong> kinds of bars are listed in results.</li>

               <li>Parameters:

                <ul>

                 <li>sellers | sources [default: sellers]

                  <ul>

                   <li>Description: Specifies whether to select countries based sellers or bean sources.</li>

                  </ul></li>

                 <li>ratings | cocoa | bars_sold [default: ratings]

                  <ul>

                   <li>Description: Specifies whether to sort by rating, cocoa percentage, or the number of different types of bars sold</li>

                  </ul></li>

                 <li>top=&lt;limit&gt; | bottom=&lt;limit&gt; [default: top=10]

                  <ul>

                   <li>Description: Specifies whether to list the top &lt;limit&gt; matches or the bottom &lt;limit&gt; matches.</li>

                  </ul></li>

                </ul></li>

              </ul></li>

            </ul></li>

          </ul></li>

        </ul></li>

      </ul></li>

    </ul></li>

  </ul></li>

</ul>




<ul>

 <li></li>

</ul>

<h3>Return Values</h3>

The return value for process_command( ) varies depending on the command issued. Matching the return values as specified is essential for passing the unit tests.




The mapping of commands to outputs is as follows:




<table width="624">

 <tbody>

  <tr>

   <td width="101"><strong>Command</strong></td>

   <td width="73"><strong>Return Value</strong></td>

   <td width="450"><strong>Source Columns for tuple values</strong></td>

  </tr>

  <tr>

   <td width="101">bars</td>

   <td width="73">6-tuple</td>

   <td width="450">‘SpecificBeanBarName’,’Company’, ‘CompanyLocation’, ‘Rating’, ‘CocoaPercent’, ‘BroadBeanOrigin’</td>

  </tr>

  <tr>

   <td width="101">companies</td>

   <td width="73">3-tuple</td>

   <td width="450">‘Company’, ‘CompanyLocation’, &lt;agg&gt;<em>Where “agg” is the requested aggregation (i.e., average rating or cocoa percent, or number of bars sold)</em></td>

  </tr>

  <tr>

   <td width="101">countries</td>

   <td width="73">3-tuple</td>

   <td width="450">‘Country’, ‘Region’, &lt;agg&gt;<em>Where “agg” is the requested aggregation (i.e., average rating or cocoa percent, or number of bars sold)</em></td>

  </tr>

  <tr>

   <td width="101">regions</td>

   <td width="73">2-tuple</td>

   <td width="450">‘Region’, &lt;agg&gt;<em>Where “agg” is the requested aggregation (i.e., average rating or cocoa percent, or number of bars sold)</em></td>

  </tr>

 </tbody>

</table>

<ul>

 <li style="list-style-type: none;">

  <ul>

   <li></li>

  </ul></li>

</ul>

<h2>Part 3: Interactive Capabilities [40 points]</h2>

Implement a command line interface to allow a user to specify queries using the language and syntax described in Part 2. The only things you’ll need to add in this part are

<ul>

 <li>prompting the user for input</li>

 <li>formatting the output “nicely”</li>

 <li>adding basic error handling (i.e., not crashing the program on invalid inputs)</li>

</ul>




Grading:

<ul>

 <li>[15 points] Graceful error handling

  <ul>

   <li>(This is only for handling <strong>invalid</strong> If your program crashes on valid inputs, you will get a 0 and need to submit again for regrade, which will be capped at 60% of the max grade. If you choose to implement different command, for example, “bars rating” instead of “bars ratings”, please update the instruction or it will be considered an error.)</li>

  </ul></li>

 <li>[20 points] Presentable output</li>

 <li>[10 points] Fixed width formatting matching (more or less) exactly the sample output below</li>

</ul>




Here is an example run:




spinoza$ python3 proj3_choc_solution.py




Enter a command: bars ratings

Chuao           Amedei          Italy           5.0  70%  Venezuela (B…

Toscano Blac… Amedei          Italy           5.0  70%  Unknown

Pablino         A. Morin        France          4.0  70%  Peru

Chuao           A. Morin        France          4.0  70%  Venezuela (B…




Chanchamayo … A. Morin        France          4.0  63%  Peru

Morobe          Amano           United State… 4.0  70%  Papua New Gu…

Guayas          Amano           United State… 4.0  70%  Ecuador

Porcelana       Amedei          Italy           4.0  70%  Venezuela (B…

Nine            Amedei          Italy           4.0  75%  Unknown

Madagascar      Amedei          Italy           4.0  70%  Madagascar




Enter a command: bars sellcountry=US cocoa bottom=5

Peru, Madaga… Ethel’s Arti… United State… 2.5  55%  Unknown

Trinidad        Ethel’s Arti… United State… 2.5  55%  Trinidad and…

O’ahu, N. Sh… Guittard        United State… 3.0  55%  United State…

O’ahu, N. Sh… Malie Kai (G… United State… 3.5  55%  United State…

O’ahu, N. Sh… Malie Kai (G… United State… 2.8  55%  United State…




Enter a command: companies region=Europe bars_sold

Bonnat          France          27

Pralus          France          25

<ol>

 <li>Morin France 23</li>

</ol>

Domori          Italy           22

Valrhona        France          21

Hotel Chocol… United Kingd… 19

Coppeneur       Germany         18

Zotter          Austria         17

Artisan du C… United Kingd… 16

Szanto Tibor    Hungary         15




Enter a command: companies ratings top=8

Amedei          Italy           3.8

Patric          United State… 3.8

Idilio (Felc… Switzerland     3.8

Benoit Nihan… Belgium         3.7

Cacao Sampak… Spain           3.7

Bar Au Choco… United State… 3.6




Soma            Canada          3.6

Brasstown ak… United State… 3.6




Enter a command: countries bars_sold

United State… Americas        764

France          Europe          156

Canada          Americas        125

United Kingd… Europe          107

Italy           Europe          63

Ecuador         Americas        55

Australia       Oceania         49

Belgium         Europe          40

Switzerland     Europe          38

Germany         Europe          35




Enter a command: countries region=Asia ratings

Viet Nam        Asia            3.4

Israel          Asia            3.2

Korea (Repub… Asia            3.2

Japan           Asia            3.1




Enter a command: regions bars_sold

Americas        1085

Europe          568

Oceania         70

Asia            46

Africa          26




Enter a command: regions ratings

Oceania         3.3

Asia            3.2

Europe          3.2

Americas        3.2

Africa          3.0




Enter a command: bad command

Command not recognized: bad command




Enter a command:




Enter a command: bars nothing

Command not recognized: bars nothing




Enter a command: exit

bye














