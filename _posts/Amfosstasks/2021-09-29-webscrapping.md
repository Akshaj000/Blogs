---
title: "WebScraping"
published: true
---
<p style="text-align:left; color:white">
<a href="/AmfossPreveshan/2021/11/19/Amfoss/index.html" >< Introspection</a>
</p>
<br/>

## [Task-07 (Webscrapping using golang)](https://github.com/Akshaj000/amfoss-tasks/tree/master/task-07)
### I SUGGEST U TO GO ALL THE WAY DOWN TO THE ACTUAL CODE.
I couldnt complete this task properly, The code pasted below is done on [bloomberg](https://www.bloomberg.com/billionaires/)
I tried the same on [forbes](https://www.forbes.com/real-time-billionaires/#131241903d78) and i couldnt succeed.

I searched for how to scrap websites using golang and most of the result came was using colly framework. I basically used the code from there and tweaked a few lines. I did try qoquery framework for forbes . It didnt help. What even worse was the method i used to fetch top 10 lines. Its actually a big mess.
### Codes i tried on forbes :
[Using colly](https://github.com/Akshaj000/amfoss-tasks/blob/master/task-07/trys(unsucessful%20ones)/try1(colly).go)\
[Using goquery](https://github.com/Akshaj000/amfoss-tasks/blob/master/task-07/trys(unsucessful%20ones)/try2(goquery).go)

### table :
```

|**Name**       |**Networth**   |**Country**    |**Field**      |
|:-------------:|:-------------:|:-------------:|:-------------:|
|Elon Musk      |$252B          |United States  | Technology    |
|Jeff Bezos     |$193B          |United States  | Technology    |
|BernardArnault |$166B          |France         | Consumer      | 
|Bill Gates     |$134B          |United States  | Technology    |
|Larry Page     |$123B          |United States  | Technology    |
|MarkZuckerberg |$121B          |United States  | Technology    |
|Sergey Brin    |$119B          |United States  | Technology    |
|Larry Ellison  |$114B          |United States  | Technology    |
|Steve Ballmer  |$111B          |United States  | Technology    |
|Warren Buffett |$105B          |United States  | Diversified   |

```
### Code :
```golang
package main

import (
	"encoding/csv"
	"fmt"
	"io"
	"log"
	"os"

	"github.com/gocolly/colly"
)

func scrap() {

	fName := "data1.csv"
	file, err := os.Create(fName)
	if err != nil {
		log.Fatalf("Could not create file, err :%q", err)
		return
	}
	defer file.Close()
	writer := csv.NewWriter(file)
	defer writer.Flush()

	c := colly.NewCollector()
	c.OnError(func(r *colly.Response, err error) {
		fmt.Println("Error:", err.Error())
	})
	c.OnResponse(func(r *colly.Response) {
		fmt.Println("visited", r.Request.URL)
	})
	c.OnHTML("div.table-row", func(e *colly.HTMLElement) {
		writer.Write([]string{
			e.ChildText("div.table-cell.t-name"),
			e.ChildText("div.table-cell.active.t-nw"),
			e.ChildText("div.table-cell.t-country"),
			e.ChildText("div.table-cell.t-industry"),
		})
		fmt.Println(e.Text)
	})

	c.OnScraped(func(r *colly.Response) {
		fmt.Println("Finished", r.Request.URL)
	})

	c.Visit("https://www.bloomberg.com/billionaires/")
}

func main() {
	scrap()
	csvfile1, err := os.Open("data1.csv")
	if err != nil {
		fmt.Println(err)
		return
	}
	defer csvfile1.Close()
	reader := csv.NewReader(csvfile1)
	csvfile2, err := os.Create("data.csv")
	if err != nil {
		fmt.Println(err)
		return
	}
	writer := csv.NewWriter(csvfile2)

	for i := 0; i < 10; i++ {
		record, err := reader.Read()
		if err != nil {
			if err == io.EOF {
				break
			}
			fmt.Println(err)
			return
		}
		err = writer.Write(record)
		if err != nil {
			fmt.Println(err)
			return
		}
	}
	writer.Flush()
	err = csvfile2.Close()
	if err != nil {
		fmt.Println(err)
		return
	}
	e := os.Remove("data1.csv")
	if e != nil {
		log.Fatal(e)
	}
}
```

## I DID IT but its complicated

This time i figured out where i went wrong and understood html codes in forbes were java script rendered. Geziyor was the framework which supported it. Its was easy after identifying it.Its possible to fetch required part of the html to a json file using this framework, and is easy to convert it to a csv.But there was issue. Go couldnt identify  class="Country/Territory". Even i tried it with "//". It didnt help. I couldnt print the Country/Territory. I couldnt come up with a btr approach. How i did it was, I used geziyor to scrap all the html codes to a txt file, editted each "Country/Territory"  to "Country". Made a local web server and used colly to scrap it. This time it worked, But i had to run two files. One with the geziyor frame work which scraped the link and made a local server. Then one with colly which actually scraps the required elements and save it inside a csv file.I should have tried to find a way to include "/" and im still working on it.
Please follow this method to use my scraper:\
1.Head to working_one directory\
2.Run the gey-serve.go file inside the serve_here directory\
3.Run the colly.go file inside the working_one directory

Code to run go file : go run . or go run filename.go

[THIS](https://github.com/Akshaj000/amfoss-tasks/tree/master/task-07/working_one) is my final file.
Im not pasting my code. Please visit my github directory.

Webscraping using geziyor is easy. Please follow this [link](https://github.com/geziyor/geziyor).

```
ParseFunc: func(g *geziyor.Geziyor, r *client.Response) {
        writer.write(string(r.Body))
    },
```
I had to write characters one by one to different file. Later i had to edit that file properly to use colly.
[This](/Htmltask/forbes/data.html) is the html file i got after scrapping. Yeah its impossible to scrap that. So removed first few lines using this [method](https://stackoverflow.com/questions/30940190/remove-first-line-from-text-file-in-golang).
Later had to convert the html to txt, since golang couldnt read "/" inside html properly due to some reason. I replaced "Country/Territory" with "Country" using :
```golang
func visit(path string, fi os.FileInfo, err error) error {

	if err != nil {
		return err
	}

	matched, _ := filepath.Match("*.txt", fi.Name())

	if matched {
		read, err := ioutil.ReadFile("out.txt")
		if err != nil {
			panic(err)
		}

		fmt.Println(path)

		newContents := strings.Replace(string(read), "Country/Territory", "Country", -1)

		fmt.Println(newContents)

		err = ioutil.WriteFile(path, []byte(newContents), 0)
		if err != nil {
			panic(err)
		}

	}

	return nil
}
```
After doing all these , [this](/Htmltask/forbes/index.html) is my forbes now.
Later i created a local server :

```golang
        port := flag.String("p", "8100", "port to serve on")
	directory := flag.String("d", ".", "the directory of static file to host")
	flag.Parse()

	http.Handle("/", http.FileServer(http.Dir(*directory)))

	log.Printf("Serving %s on HTTP port: %s\n", *directory, *port)
	log.Fatal(http.ListenAndServe(":"+*port, nil))
```
Used basic colly to scrap from here. And added to csv. Its complicated but yeah finally i did it. 

CSV file :
```
Country,NetWorth,Age,Source
United States,$271.5 B,50,"Tesla, SpaceX"
United States,$203.1 B,57,Amazon
France,$199.5 B,72,LVMH
United States,$138.8 B,66,Microsoft
United States,$127.0 B,48,Google
United States,$126.1 B,77,software
United States,$122.3 B,48,Google
United States,$121.9 B,37,Facebook
United States,$106.0 B,65,Microsoft
United States,$104.1 B,91,Berkshire Hathaway

```

# Finally
I should have actually found a different way to type /.
Im supposed to use ```'\\/'``` instead of ```/``` or ```//```.


# THE ACTUAL WORKING CODE :
```golang
package main

import (
	"encoding/csv"
	"os"

	"github.com/PuerkitoBio/goquery"
	"github.com/geziyor/geziyor"
	"github.com/geziyor/geziyor/client"
)

func main() {

	file, _ := os.Create("data.csv")
	writer := csv.NewWriter(file)
	head := []string{"Country", "Networth", "Age", "Source"}
	writer.Write(head)
	count := 0
	geziyor.NewGeziyor(&geziyor.Options{
		StartRequestsFunc: func(g *geziyor.Geziyor) {
			g.GetRendered("https://www.forbes.com/real-time-billionaires/#50cd30c23d78", g.Opt.ParseFunc)
		},
		ParseFunc: func(g *geziyor.Geziyor, r *client.Response) {
			r.HTMLDoc.Find("tr.base.ng-scope").Each(func(_ int, s *goquery.Selection) {
				Country := s.Find("td.Country\\/Territory").Text()
				Networth := s.Find("td.Net.Worth").Text()
				Age := s.Find("td.age").Text()
				Source := s.Find("td.source").Text()
				posts := []string{Country, Networth, Age, Source}
				writer.Write(posts)
				count += 1
				if count <= 10 {
					writer.Flush()
				}
			})

		},
	}).Start()
}

```
CSV FILE :
```
Country,NetWorth,Age,Source
United States,$271.5 B,50,"Tesla, SpaceX"
United States,$203.1 B,57,Amazon
France,$199.5 B,72,LVMH
United States,$138.8 B,66,Microsoft
United States,$127.0 B,48,Google
United States,$126.1 B,77,software
United States,$122.3 B,48,Google
United States,$121.9 B,37,Facebook
United States,$106.0 B,65,Microsoft
United States,$104.1 B,91,Berkshire Hathaway
```




<br/>
<p style="text-align:left;">
<a href="/AmfossPreveshan/2021/09/28/flutter/index.html" >< Previous</a>
<span style="float:right;"><a href="/AmfossPreveshan/2021/09/30/htmljs/index.html" >Next ></a>
</span>
</p>

