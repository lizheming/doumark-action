![](assets/douban.png)
# Douban sync for GitHub Actions

[GitHub Action](https://github.com/features/actions) for douban movie/book/music marked data sync to csv file or notion automatically.

## Input variables

See [action.yml](action.yml) for more detailed information.
- id: Douban ID
- type: Douban data Type, enum value: movie, book, music, default `movie`
- format: Douban data store format, enum value：csv, json, notion, default `csv`
- dir: Target where douban data sync to. It's a file path for `csv` and `json` format, and a notion database id for `notion` format. 
- notion_token: Notion Integration Token
## Usage

### Sync to CSV file

```yml
# .github/workflows/douban.yml
name: douban
on: 
  schedule:
  - cron: "30 * * * *"

jobs:
  douban:
    name: Douban mark data sync
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: movie
      uses: lizheming/doumark-action@master
      with:
        id: lizheming
        type: movie
        format: csv
        dir: ./data/douban

    - name: music
      uses: lizheming/doumark-action@master
      with:
        id: lizheming
        type: music
        format: csv
        dir: ./data/douban
  
    - name: Commit
      uses: EndBug/add-and-commit@v8
      with:
        message: 'chore: update douban data'
        add: './data/douban'
```
### Sync to Notion

1. Create a Notion Integration at [My Integrations - Notion](https://www.notion.so/my-integrations). And here you can get `NOTION_TOKEN`.
    - Associated workspace: You should select workspace which you should store.
    - Capabilities: Both of `Read`, `Update` and `Insert` content abilities shoud checked.
2. Duplicate database by click <kbd>Duplicate</kbd> at the top right postion of <[Movie](https://lizheming.notion.site/d8a363df3ca84ca89ef52208ad874e3b) | [Book](https://lizheming.notion.site/488c17fd89fb424591f68f7cfb029020) | [Music](https://lizheming.notion.site/d80ca60213c54ab99c4376caec0be9d7)> page.
3. Share database to your Integration by inviting it with <kbd>Share</kbd> - <kbd>Invite</kbd> at the top right postion. And you can get database id, the first random string from url.

```yml
# .github/workflows/douban.yml
name: douban
on: 
  schedule:
  - cron: "30 * * * *"

jobs:
  douban:
    name: Douban mark data sync
    runs-on: ubuntu-latest
    steps:
    - name: movie
      uses: lizheming/doumark-action@master
      with:
        id: lizheming
        type: movie
        format: notion
        dir: xxxx
        notion_token: ${{ secrets.notion_token }}
        
    - name: music
      uses: lizheming/doumark-action@master
      with:
        id: lizheming
        type: music
        format: notion
        dir: xxxx
        notion_token: 
        notion_token: ${{ secrets.notion_token }}
```