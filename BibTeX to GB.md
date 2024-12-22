# BibTeX to GB/T 7714-2015 Conversion Example

## BibTeX Format
```bibtex
@inproceedings{DBLP:conf/kdd/AggarwalLWW09,
  author       = {Charu C. Aggarwal and
                  Yan Li and
                  Jianyong Wang and
                  Jing Wang},
  editor       = {John F. Elder IV and
                  Fran{\c{c}}oise Fogelman{-}Souli{\'{e}} and
                  Peter A. Flach and
                  Mohammed Javeed Zaki},
  title        = {Frequent pattern mining with uncertain data},
  booktitle    = {Proceedings of the 15th {ACM} {SIGKDD} International Conference on
                  Knowledge Discovery and Data Mining, Paris, France, June 28 - July
                  1, 2009},
  pages        = {29--38},
  publisher    = {{ACM}},
  year         = {2009},
  url          = {https://doi.org/10.1145/1557019.1557030},
  doi          = {10.1145/1557019.1557030},
  timestamp    = {Thu, 28 Jan 2021 11:41:18 +0100},
  biburl       = {https://dblp.org/rec/conf/kdd/AggarwalLWW09.bib},
  bibsource    = {dblp computer science bibliography, https://dblp.org}
}
```

## Conversion Process to GB/T 7714-2015
1. **Author**:
   - Extract the `author` field.
   - Format names as "Last name, First name initials".
   - Separate multiple authors with commas.
   - Example: `Charu C. Aggarwal, Yan Li, Jianyong Wang, Jing Wang` becomes:
     
     ```markdown
     Aggarwal, C.C., Li, Y., Wang, J.Y., Wang, J.
     ```

2. **Title**:
   - Extract the `title` field.
   - Remove special BibTeX formatting, e.g., `{}` and `\`.
   - Capitalization should match the original.
   - Example: `Frequent pattern mining with uncertain data`.

3. **Conference Name**:
   - Use `booktitle`.
   - Format with italics for the conference name.
   - Include location and date from `booktitle` if provided.
   - Example: `Proceedings of the 15th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, Paris, France, June 28 - July 1, 2009` becomes:
     
     ```markdown
     Proceedings of the 15th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, Paris, France, 2009.
     ```

4. **Pages**:
   - Extract the `pages` field.
   - Example: `29--38` becomes `29-38`.

5. **Publisher**:
   - Extract `publisher`.
   - Example: `ACM`.

6. **Year**:
   - Extract `year` field.
   - Example: `2009`.

7. **DOI**:
   - Extract `doi` field.
   - Format as `DOI:` followed by the value.
   - Example: `DOI: 10.1145/1557019.1557030`.

## Final GB/T 7714-2015 Format
```markdown
[1] Aggarwal, C.C., Li, Y., Wang, J.Y., Wang, J. Frequent pattern mining with uncertain data[C]. Proceedings of the 15th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining, Paris, France, 2009: 29-38. DOI: 10.1145/1557019.1557030.
```

