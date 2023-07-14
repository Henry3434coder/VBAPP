## SQL Query for the Report Page

```sql
# The passing stats page is a classic report page.
# SQL query for this report page is:

SELECT
    p.PLAYER_ID,
    p.NAME,
    p.POSITION,
    p.CLASS,
    p.JERSEY_NUMBER,
    ps.IN_SYSTEM_PASSES,
    ps.OUT_OF_SYSTEM_PASSES,
    CASE WHEN (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES) = 0 THEN NULL
        ELSE ROUND((ps.IN_SYSTEM_PASSES / (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES)) * 100, 2)
    END AS PERCENT_GOOD,
    CASE WHEN (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES) = 0 THEN NULL
        ELSE ROUND((ps.OUT_OF_SYSTEM_PASSES / (ps.IN_SYSTEM_PASSES + ps.OUT_OF_SYSTEM_PASSES)) * 100, 2)
    END AS PERCENT_BAD
FROM
    PLAYER p
JOIN
    PASSING_STATS ps ON p.PLAYER_ID = ps.PLAYER_ID;
```

## Buttons for Inserting Data

To create a hidden page item to store the ID of the row:
```html
# The next most important thing to do is to create a page item. 
# This will be a hidden page item and its purpose is to store the ID of the row, in order to insert data. Mine is called P1_TASK_TO_DELETE.
```

Button for in-system passes:
```html
### Button 1: Insystem Passes
<button type="button" data-task-id="#PLAYER_ID#" class="t-Button t-Button--hot cc-delete-btn">INSYSPASS</button>
```

Button for out-of-system passes:
```html
### Button 2: Outsystem Passes
<button type="button" data-task-id="#PLAYER_ID#" class="t-Button t-Button--hot t-Button--danger cc-outsys-btn">OUTSYSPASS</button>
```

## Dynamic Actions for the Buttons

### Insystem Passes Button

1. Create a new "Click" dynamic action.
   - jQuery selector: `.cc-delete-btn`

2. True action:
   - Action: Execute JavaScript Code
     ```javascript
     let taskIdToDelete = $(this.triggeringElement).data('task-id');
     console.log(taskIdToDelete);
     apex.item('P1_TASK_TO_DELETE').setValue(taskIdToDelete);
     ```

3. True action:
   - Action: Execute PL/SQL Code
     ```sql
     BEGIN
         UPDATE passing_stats
         SET in_system_passes = in_system_passes + 1
         WHERE player_id = TO_NUMBER(:P1_TASK_TO_DELETE);
     END;
     ```
   - Items to Submit: `P1_TASK_TO_DELETE` (hidden item)

4. Refresh action to refresh the report after data entry.

### Outsystem Passes Button

1. Create a new "Click" dynamic action.
   - jQuery selector: `.cc-outsys-btn`

2. True action:
   - Action: Execute JavaScript Code
     ```javascript
     let outOfSys = $(this.triggeringElement).data('task-id');
     console.log(outOfSys);
     apex.item('P1_TASK_TO_DELETE').setValue(outOfSys);
     ```

3. True action:
   - Action: Execute PL/SQL Code
     ```sql
     BEGIN
         UPDATE passing_stats
         SET out_of_system_passes = out_of_system_passes + 1
         WHERE player_id = TO_NUMBER(:P1_TASK_TO_DELETE);
     END;
     ```
   - Items to Submit: `P1_TASK_TO_DELETE` (hidden item)

4. Refresh action to refresh the report after data entry.

## Button to Clear Passing Data

### Reset Button
```sql
# Last but not least, a button that clears the report from all passing data.
<button type="button" class="reset-btn">Reset</button>

# SQL code for the reset button:
UPDATE passing_stats
SET in_system_passes = 0,
    out_of_system_passes = 0;
```

5. Refresh action to show the reset on the page.
