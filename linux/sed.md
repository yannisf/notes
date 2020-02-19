# sed

- Delete lines that match pattern

      $ sed -i '/pattern/d' <file>

- Delete a line by number (inplace)

      $ sed -i -e '10d' <file>
      
- Delete a line range by line numbers (inplace)

      $ sed -i -e '5,10d' <file>
      
