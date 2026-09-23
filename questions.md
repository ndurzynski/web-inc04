## Undergraduate Question 1:

- The ingredients should use a column. Setting flex-direction: column works, and so does leaving the list as a normal block list.
The widest item is "3 cups quick-cooking or old-fashioned oats". At 16px Helvetica it is about 300px on one line. At 320px, .content takes 30px of left margin and the ``<ul>`` takes about 40px of padding, which leaves about 250px. That item can't fit on one line, so it will wrap onto two lines.
A single row with nowrap fails. It puts all 14 items on one line. Flex items can't shrink below their longest word, so the row overflows and the page scrolls horizontally
A wrapping row with no flex-basis starts each item at its text width. The short items like "2 eggs" and "1 cup butter" pair up on a line. The oats item gets a line to itself and is still squeezed to 250px. The lines come out ragged and uneven, which makes the list hard to scan. It is readable but messy.
A column gives each item the full 250px. Only the oats item wraps, and it wraps inside its own line. Nothing overflows, and the list reads cleanly from top to bottom.
The existing rule .ingredients { flex-basis: 45% } does nothing right now. .main-content isn't a flex container, so the rule has no effect until the parent becomes one.
The tradeoff shows up at wider viewports. A column list keeps one item per line even at 1200px, which leaves a long, narrow list with empty space beside it. A media query would have to switch it to a wrapping row or to columns: 2 on wide screens.

## Undergraduate Question 2:

- The fault is in the grid track definition. The container is fine, since it uses display: grid and justify-content: center. The text-align: center on each square only positions text inside its cell.
To check this in the browser, open DevTools, turn on the Grid overlay for #board, and put a long label like "XXXX" in one square. With fixed 100px tracks, the tracks do not grow. The overlay stays at 3 x 100px and the text overflows past the cell border. With 1fr or auto tracks, the overlay shows that one column widening to fit the text. This happens because 1fr means minmax(auto, 1fr), and its auto minimum is the content's min-content width. The whole board widens with that column.
The smallest fix keeps the fixed tracks and stops the content from pushing on them:
   #board > div { min-width: 0; overflow: hidden; }
For flexible tracks, use grid-template-columns: repeat(3, minmax(0, 1fr)); instead.
This keeps a centered 3x3 board because the track definition alone sets the track sizes. The board gets three equal columns no matter what the squares contain. Its total width stays at 3 x 100 + 2 x 10 = 320px, so justify-content: center still centers it. The long label gets clipped inside its own cell and does not resize the grid.