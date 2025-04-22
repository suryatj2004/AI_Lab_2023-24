# Ex.No: 9  Logic Programming –  Computer Maintenance Expert System
### DATE:  22/04/2025                                                                        
### REGISTER NUMBER : 212222040075
### AIM: 
Write a Prolog program to build a computer maintenance expert system.
###  Algorithm:
1. Start the program.
2. Write the rules for each fault in computer.
3. If system have printing problem, missing dots and no uniform printing then system fault on printer head.
4. If system have not printing, missing dots and spread inks then system fault on ribbon
5. If system have not printing, paper jam and out of paper then system fault on paper stuck in printer
6. Similarly define rules for all faults.
7. Define facts for system problems.
8. Find the fault of computer by passing query to system.
     
### Program:
```
diagnose(printer_head, 'Check printer head: printing problem with missing dots and non-uniform printing.') :-
    symptom(printing_problem),
    symptom(missing_dots),
    symptom(no_uniform_printing).

diagnose(ribbon, 'Ribbon issue: not printing, missing dots, and spread inks.') :-
    symptom(not_printing),
    symptom(missing_dots),
    symptom(spread_inks).

diagnose(paper_stuck_in_printer, 'Paper stuck or out of paper: not printing, paper jam and out of paper.') :-
    symptom(not_printing),
    symptom(paper_jam),
    symptom(out_of_paper).

diagnose(scanner_cable, 'Scanner cable issue: scanner not detected and loose connection.') :-
    symptom(scanner_not_detected),
    symptom(loose_connection).

diagnose(keyboard_fault, 'Keyboard issue: not responding and LED not blinking.') :-
    symptom(keyboard_not_responding),
    symptom(led_not_blinking).

symptom(printing_problem).
symptom(missing_dots).
symptom(no_uniform_printing).

start :-
    writeln('--- Welcome to the Computer Maintenance Expert System ---'),
    ( diagnose(Fault, What) ->
        format('Diagnosis: Fault = ~w~nExplanation: ~w~n', [Fault, What])
    ;
        writeln('No fault matched the current symptoms.')
    ).
```
### Output:

![image](https://github.com/user-attachments/assets/b7b204cc-8694-47b0-8b1d-2a243f577950)

### Result:
Thus the simple omputer maintenance expert system was built sucessfully.
