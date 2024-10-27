<%*
//get selection
noteContent = tp.file.selection();
//get array of lines
lines = noteContent.split('\n')
//
let newContent = "";
lines.forEach(l => {
    newContent += '' + l + "\n";
})
//
newContent = newContent.replace(/\n$/, "");
//assign the code block language - change {css} to {html}, {php}, etc.
header = "```powershell"+ "\n"
//place cursor after last selected character and add final backticks 
return header + newContent + "\n" + "```";
%>