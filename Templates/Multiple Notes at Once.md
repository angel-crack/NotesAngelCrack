``` python
python -c "[open(f'{note}.md', 'w').close() for note in ['Note 1', 'Note 2', 'Note 3']]"

python -c "[open(f'{str(i+5).zfill(2)}. {note}.md', 'w').close() for i, note in enumerate(['Mongo DB Compass', 'BDB Ts', 'Swa'])]"
```

Create files at once, "str(i+5)" file will start with "05. "

