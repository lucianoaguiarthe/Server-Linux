Theme: notes
Palet: Green
Size: Wide

---
Layout: SectionTitle
# Luciano de Aguiar Monteiro
## Tables, Math, Code, Notes,...

---
Layout: HeaderAndColumns

# Teste
+++
### Prioridade
+++
### teste
+++
### teste




---
# Tables
- To add a table, use three or more hyphens (`---`) to create each column’s header, and use pipes (`|`) to separate each column. You can optionally add pipes on either end of the table.
- It is recommended to use the table creator using the ✚ button to create a new table.
- Inline markdown can also be use incide cells
- `:` around column’s header are used to specify the column aligment.

| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | **Title**   | Here's this   |
| Paragraph   | Text        | And more      |


---
# Footnotes

- Footnotes can only be used in layouts supporting them (they need to define where footnotes will be rendered)
- Here is some text containing a footnote. [^somesamplefootnote]
- You can have multiple footnotes, even in different part of the slide.
- In the same slide, footnote identifiers need to be unique but you can reue them in a different slide.

[^somesamplefootnote]: Here is the text of the footnote itself.


---
# Blockquotes
- Blockquotes are defined by a line starting with the greater than (`>`) sign. Blockquotes can contains Markdown (bold, …)
- There is no Markdown syntax that allow us to cite an author. Instead we have to rely on HTML by using the `<cite>` tag.

> Your time is limited, so don't waste it living someone else's life. Don't be trapped by dogma – which is living with the results of other people's thinking.  
> <cite>-- Steve Jobs</cite>


--- 
# Math

Maths are rendered using [KaTeX](https://katex.org/) which is based on Donald Knuth’s TeX, **the gold standard for math typesetting**.

### Math Block

$$
 \Gamma(t) = \lim_{n \to \infty} \frac{n! \; n^t}{t \; (t+1)\cdots(t+n)}= \frac{1}{t} \prod_{n=1}^\infty \frac{\left(1+\frac{1}{n}\right)^t}{1+\frac{t}{n}} = \frac{e^{-\gamma t}}{t} \prod_{n=1}^\infty \left(1 + \frac{t}{n}\right)^{-1} e^{\frac{t}{n}}  
$$ 

is the **Gamma** Function.

### Inline math
Example:  $$k_{n+1} = n^2 + k_n^2 - k_{n-1}$$ 



---
# Code

Like Math, we can have `inline code` and *code blocks*

## Code example

```
-(void)mouseDown:(NSEvent *)theEvent {
    [super mouseDown:theEvent];
    // check for click count above one, which we assume means it's a double click event
    if([theEvent clickCount] > 1) { 
    //NSLog(@"double click!");

    if(delegate &&  [delegate  respondsTySelector:@selector(doubleClick:)]) { 
            [delegate performSelector:@selector(doubleClick:) withObject:self]; 
        } 
    }
}
```


--- 
# Notes

^ You can add notes to your slides. Notes are not exported in the slide but are visible in the presenter when you present.
^ - A note is a line starting by `^`
^ - You can add multiples lines of notes inside the same slide.
^ - There is no need to group the notes lines, neither to have then at the end of the slide. Slideas will extract and group them during conversion.
^ - Notes can contains basic Markdown formating such as lists, bold, italic,…

All the above text is not visible in the slide