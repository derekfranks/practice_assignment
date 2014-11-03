##FAQs
I thought I would pull together some frequently asked questions regarding Programming Assignment 1 that regularly show up on the forums.

####1) My code runs fine and my answers match the sample output, but whenever I try to submit, I get a message telling me that my answer is incorrect.

You're not submitting via the Coursera website are you?  You need to re-read the Assignment 1 instructions (all of them).  

Instead of submitting via the website, you need to use the `submit()` script.  A link and more detailed instructions are included in the "Grading" section of the assignment 1 instructions.

If you're using the `submit()` script and still getting this error, double check to make sure you're not printing the results rather than returning them.  In other words, the final line of your code should not contain `print()`.  Use `return()` instead (you may not have to do this as R automagically returns the final line of a function).

####2) Do I need to round my answers to match the sample output?

No.  You don't need to do any rounding of your results to pass the submission tests.



####3) I only see 3 parts to the assignment.  What are these 10 parts listed on the assignment page?

You're correct that there are only 3 parts to assignment 1.  The 10 parts could probably be more accurately described as tests.  The `submit()` script will run your code with a variety of different parameters to test it.  If there are issues with your function, it may only pass some of the tests.



####4) My `pollutantmean()` passes the first 3 tests, but fails the 4th with the error message: "Error in pollutantmean("specdata", "nitrate") : argument "id" is missing, with no default"
  
You didn't assign a default value to `id`.  The first line of your function should look exactly like the one in the instructions:  `pollutantmean <- function(directory, pollutant, id = 1:332) {`



####5) I get an error stating "unexpected '>' " or "unexpected '{' ".

You probably have an open `(` somewhere in your code.  Double check it with a fine tooth comb to make sure you've closed all of your `()`, `{}`and `[]`.  



####6) My code seems to work but my answers don't match the sample output.

Are you calculating the mean value for each file and then taking the mean of those means?  That's not the correct approach.  You need to combine all of the relevant data into a single data frame or vector and take the mean of *that*.
 
    a <- c(1:5)
    b <- c(1:10)
    c <- c(10:15)
    
    mean(c(a,b,c))
    [1] 6.904762

    mean(c(mean(a),mean(b),mean(c)))
    [1] 7

You want the first approach, not the second.



####7) My function seems to work when `id` is a single value but I get the following error message when it's something like `70:72`: "In pollutant1$ID == 1:332 :  longer object length is not a multiple of shorter object length".

You probably have a line of code that looks something like this:  `dat_subset <- dat[which(dat[ , "ID"] == id), ]`

Subsetting by `ID` works when `id` is a vector of length 1. However, when `id = 1:10` for example, you have a problem. The issue goes back to the SWIRL example (and maybe lecture?) regarding how R handles vectors of differing lengths.

An example:

    > x <- 1:10
    > y <- 1:5
    > x + y
     [1]  2  4  6  8 10  7  9 11 13 15
	 
Each value of y gets added to x. But because y is shorter than x, after adding 5+5, R starts over from the beginning of y and adds 6+1, 7+2, etc.

That's what is happening with the subset when id is a vector longer than 1. The first row gets compared to the first value of id. The second row gets compared to the second value of id, etc. It repeats id until it gets to the end of the vector or data frame you're subsetting. The warning is telling you that the length of dat$nitrate or dat$sulfate is not divisible by the length of id.

Essentially, there are 2 options to solve this.  The first is to not use a subset for `id` at all.  You presumably already have a loop in your code, so instead of combining all 332 files together, why not use that loop to combine only the files specified by `id` in the first place?

The other alternative is to replace the `==` with `%in%`.  In this case, the %in% operator will check each value of `id` against every value in the `ID` column, which is what you want.  The downside to this approach is that it will probably be very, very slow if you've followed the tutorial example to create `pollutantmean()`.



####8) How do I subset for either `nitrate` or `sulfate` when I calculate the mean?

If you wanted to subset nitrate, you would do that with `dat[, "nitrate"]`. Likewise you would use `dat[, "sulfate"]` for sulfate. When the function gets called you'll have something like: `pollutantmean(directory = "specdata", pollutant = "nitrate", id = 1:332)`.

So if you have either `pollutant = "nitrate"` or `pollutant = "sulfate"`, what would you put in place of `"sulfate"` and `"nitrate"` in subsetting examples above so that it would work in either case?



####9) I'm subsetting my data frame using `dat$pollutant` but it doesn't seem to be working.

Recall from the lectures that $ makes R look for a literal name match. That's not what you want. You want to subset by the value of pollutant (either "sulfate" or "nitrate"), not by "pollutant" since you don't have a column by that name. So, you need to use brackets instead of $.

That could be either something like [[pollutant]] or [, pollutant]

