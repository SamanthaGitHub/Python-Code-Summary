# Python Internship Code Summary
## Introduction
During my internship at Prosper IT Consulting, I worked with team members to develop a travel guide web application in Python. Using the Django Framework and PyCharm IDE, the application we created is a travel guide to Japan. I worked along side a team of other student developers using Azure DevOps and Agile methodology. The project consisted of ten stories during a two-week long sprint with daily standup meetings and weekly code retrospectives.

Featured below is my code that shows some of the development and functionality of my application.

![images](./images/home.JPG)

## CRUD Functionality
### Create
The first task was to create a model and function-based view for adding a new place to the travel guide database.

```
Function:
def traveljapan_create(request):
    form = AddEntryForm(data=request.POST or None)
    if request.method == 'POST':
        if form.is_valid():
            form.save()
            return redirect('../places')
    content = {'form': form}
    return render(request, 'TravelJapan/TravelJapan_create.html', content)


Model:
islands = [('Hokkaido', 'Hokkaido'), ('Honshu', 'Honshu'), ('Kyushu', 'Kyushu'),
           ('Shikoku', 'Shikoku'), ('Okinawa', 'Okinawa')]
addTypes = [('Restaurant', 'Restaurant'), ('Sightseeing', 'Sightseeing'), ('Festival', 'Festival'), ('Other', 'Other')]


class AddEntry(models.Model):
    name_of_Place = models.CharField(max_length=50)
    city = models.CharField(max_length=30)
    island = models.CharField(max_length=8, choices=islands)
    activity_Type = models.CharField(max_length=15, choices=addTypes)
    description = models.TextField(max_length=500)
    addDate = models.DateTimeField(auto_now_add=True)

    AddEntries = models.Manager()

    def __str__(self):
        return self.name_of_Place
```

### Read
To view an item, I developed two functions; one that displays all items in the database on a details template and another to display all the information about a particular item.

```
def traveljapan_places(request):
    addEntry = AddEntry.AddEntries.all()
    content = {'addEntry': addEntry}
    return render(request, 'TravelJapan/TravelJapan_places.html', content)

def traveljapan_details(request, pk):
    addEntry = get_object_or_404(AddEntry, pk=pk)
    content = {'addEntry': addEntry}
    return render(request, 'TravelJapan/TravelJapan_details.html', content)
```

### Update and Delete
From the details template, a user can update or delete an entry. The update function offers the option to save changes or delete an item. If the user choses to delete, a delete confirmation page will render to be sure the user does in fact want to delete.

```
def traveljapan_update(request, pk):
    addEntry = get_object_or_404(AddEntry, pk=pk)
    form = AddEntryForm(data=request.POST or None, instance=addEntry)
    if request.method == 'POST':
        if form.is_valid():
            form.save()
            return redirect('../../places')
    content = {'form': form, 'addEntry': addEntry}
    return render(request, 'TravelJapan/TravelJapan_update.html', content)

def traveljapan_delete(request, pk):
    addEntry = get_object_or_404(AddEntry, pk=pk)
    if request.method == 'POST':
        addEntry.delete()
        return redirect('../../places')
    content = {'addEntry': addEntry}
    return render(request, 'TravelJapan/TravelJapan_delete.html', content)
```

## Web Scraping
### Beautiful Soup
To display a list of attractions from a website, I used Beautiful Soup and scraped the headers for the names of the attractions.

```
def traveljapan_bs(request):
    site = requests.get('https://www.planetware.com/tourist-attractions/japan-jpn.htm')
    soup = BeautifulSoup(site.content, 'html.parser')
    placelist = []
    info = soup.find_all('h2', class_='sitename')
    for i in info:
        placelist += list(i)

    content = {'placelist': placelist}
    return render(request, 'TravelJapan/TravelJapan_bs.html', content)
```

#### Save
[under construction]

## API
I parsed through a JSON response from the [Kanji Alive](https://app.kanjialive.com/api/docs) API on RapidAPI to display a kanji character and it's english meaning.

```
def traveljapan_api(request):
    url = "https://kanjialive-api.p.rapidapi.com/api/public/kanji/%E8%A8%AA"

    headers = {
        "X-RapidAPI-Key": {secret_API_key}
        "X-RapidAPI-Host": "kanjialive-api.p.rapidapi.com"
    }

    response = requests.request("GET", url, headers=headers)
    kanji = json.loads(response.text)

    character = kanji["kanji"]["character"]
    meaning = kanji["kanji"]["meaning"]["english"]

    content = {"character": character, "meaning": meaning}

    return render(request, 'TravelJapan/TravelJapan_api.html', content)
```

## Skills Aquired
### <b>Version Control</b>
Git Version control was used extensively, as all team members worked off of local versions of the same remote repository. I created branches for each of the ten stories and successfully pushed them to the master without merge conflicts, giving me much experience in a professional work environment. I also much more appreciate version control capabilities having had more time and experience in using it during the sprint.

### <b>Researching/Troubleshooting</b>
When running into roadblocks, I searched online for answers. By using numerous different keywords and looking at different search results I became more adept at finding exactly what I was after. I also learned how useful looking at a variety of answers to a problem is, where I could cherry-pick different aspects I wanted to implement into my code.
