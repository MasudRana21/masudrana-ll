% vim: set textwidth=120:

% Example CV based on the 1.5-column-cv template. Main features:
% * uses the Roboto font family and IcoMoon icon set;
% * doesn't use colours, different font weights are used instead for styling;
% * because the CV fits on one page, header and footer is empty, since there isn't much useful info to put there;
% * includes a photo.
\documentclass[a4paper,10pt]{article}


% package imports
% ---------------

\usepackage[british]{babel} % for correct language and hyphenation and stuff
\usepackage{calc}           % for easier length calculations (infix notation)
\usepackage{enumitem}       % for configuring list environments
\usepackage{fancyhdr}       % for setting header and footer
\usepackage{fontspec}       % for fonts
\usepackage{geometry}       % for setting margins (\newgeometry)
\usepackage{graphicx}       % for pictures
\usepackage{microtype}      % for microtypography stuff
\usepackage{xcolor}         % for colours


% margin and column widths
% ------------------------

% margins
\newgeometry{left=15mm,right=15mm,top=15mm,bottom=15mm}

% width of the gap between left and right column
\newlength{\cvcolumngapwidth}
\setlength{\cvcolumngapwidth}{3.5mm}

% left column width
\newlength{\cvleftcolumnwidth}
\setlength{\cvleftcolumnwidth}{44.5mm}

% right column width
\newlength{\cvrightcolumnwidth}
\setlength{\cvrightcolumnwidth}{\textwidth-\cvleftcolumnwidth-\cvcolumngapwidth}

% set paragraph indentation to 0, because it screws up the whole layout otherwise
\setlength{\parindent}{0mm}


% style definitions
% -----------------
% style categories explanation:
% * \cvnameXXX is used for the name;
% * \cvsectionXXX is used for section names (left column, accompanied by a horizontal rule);
% * \cvtitleXXX is used for job/education titles (right column);
% * \cvdurationXXX is used for job/education durations (left column);
% * \cvheadingXXX is used for headings (left column);
% * \cvmainXXX (and \setmainfont) is used for main text;
% * \cvruleXXX is used for the horizontal rules denoting sections.

% font families
\defaultfontfeatures{Ligatures=TeX} % reportedly a good idea, see https://tex.stackexchange.com/a/37251

\newfontfamily{\cvnamefont}{Roboto Medium}
\newfontfamily{\cvsectionfont}{Roboto Medium}
\newfontfamily{\cvtitlefont}{Roboto Regular}
\newfontfamily{\cvdurationfont}{Roboto Light Italic}
\newfontfamily{\cvheadingfont}{Roboto Regular}
\setmainfont{Roboto Light}

% colours
\definecolor{cvnamecolor}{HTML}{000000}
\definecolor{cvsectioncolor}{HTML}{000000}
\definecolor{cvtitlecolor}{HTML}{000000}
\definecolor{cvdurationcolor}{HTML}{000000}
\definecolor{cvheadingcolor}{HTML}{000000}
\definecolor{cvmaincolor}{HTML}{000000}
\definecolor{cvrulecolor}{HTML}{000000}

\color{cvmaincolor}

% styles
\newcommand{\cvnamestyle}[1]{{\Large\cvnamefont\textcolor{cvnamecolor}{#1}}}
\newcommand{\cvsectionstyle}[1]{{\normalsize\cvsectionfont\textcolor{cvsectioncolor}{#1}}}
\newcommand{\cvtitlestyle}[1]{{\large\cvtitlefont\textcolor{cvtitlecolor}{#1}}}
\newcommand{\cvdurationstyle}[1]{{\small\cvdurationfont\textcolor{cvdurationcolor}{#1}}}
\newcommand{\cvheadingstyle}[1]{{\normalsize\cvheadingfont\textcolor{cvheadingcolor}{#1}}}


% inter-item spacing
% ------------------

% vertical space after personal info and standard CV items
\newlength{\cvafteritemskipamount}
\setlength{\cvafteritemskipamount}{5mm plus 1.25mm minus 1.25mm}

% vertical space after sections
\newlength{\cvaftersectionskipamount}
\setlength{\cvaftersectionskipamount}{2mm plus 0.5mm minus 0.5mm}

% extra vertical space to be used when a section starts with an item with a heading (e.g. in the skills section),
% so that the heading does not follow the section name too closely
\newlength{\cvbetweensectionandheadingextraskipamount}
\setlength{\cvbetweensectionandheadingextraskipamount}{1mm plus 0.25mm minus 0.25mm}


% intra-item spacing
% ------------------

% vertical space after name
\newlength{\cvafternameskipamount}
\setlength{\cvafternameskipamount}{3mm plus 0.75mm minus 0.75mm}

% vertical space after personal info lines
\newlength{\cvafterpersonalinfolineskipamount}
\setlength{\cvafterpersonalinfolineskipamount}{2mm plus 0.5mm minus 0.5mm}

% vertical space after titles
\newlength{\cvaftertitleskipamount}
\setlength{\cvaftertitleskipamount}{1mm plus 0.25mm minus 0.25mm}

% value to be used as parskip in right column of CV items and itemsep in lists (same for both, for consistency)
\newlength{\cvparskip}
\setlength{\cvparskip}{0.5mm plus 0.125mm minus 0.125mm}

% set global list configuration (use parskip as itemsep, and no separation otherwise)
\setlist{parsep=0mm,topsep=0mm,partopsep=0mm,itemsep=\cvparskip}


% CV commands
% -----------

% creates a "personal info" CV item with the given left and right column contents, with appropriate vertical space after
% @param #1 left column content (should be the CV photo)
% @param #2 right column content (should be the name and personal info)
\newcommand{\cvpersonalinfo}[2]{
    % left and right column
    \begin{minipage}[t]{\cvleftcolumnwidth}
        \vspace{0mm} % XXX hack to align to top, see https://tex.stackexchange.com/a/11632
        \raggedleft #1
    \end{minipage}% XXX necessary comment to avoid unwanted space
    \hspace{\cvcolumngapwidth}% XXX necessary comment to avoid unwanted space
    \begin{minipage}[t]{\cvrightcolumnwidth}
        \vspace{0mm} % XXX hack to align to top, see https://tex.stackexchange.com/a/11632
        #2
    \end{minipage}

    % space after
    \vspace{\cvafteritemskipamount}
}

% typesets a name, with appropriate vertical space after
% @param #1 name text
\newcommand{\cvname}[1]{
    % name
    \cvnamestyle{#1}

    % space after
    \vspace{\cvafternameskipamount}
}

% typesets a line of personal info beginning with an icon, with appropriate vertical space after
% @param #1 parameters for the \includegraphics command used to include the icon
% @param #2 icon filename
% @param #3 line text
\newcommand{\cvpersonalinfolinewithicon}[3]{
    % icon, vertically aligned with text (see https://tex.stackexchange.com/a/129463)
    \raisebox{.5\fontcharht\font`E-.5\height}{\includegraphics[#1]{#2}}
    % text
    #3

    % space after
    \vspace{\cvafterpersonalinfolineskipamount}
}

% creates a "section" CV item with the given left column content, a horizontal rule in the right column, and with
% appropriate vertical space after
% @param #1 left column content (should be the section name)
\newcommand{\cvsection}[1]{
    % left and right column
    \begin{minipage}[t]{\cvleftcolumnwidth}
        \raggedleft\cvsectionstyle{#1}
    \end{minipage}% XXX necessary comment to avoid unwanted space
    \hspace{\cvcolumngapwidth}% XXX necessary comment to avoid unwanted space
    \begin{minipage}[t]{\cvrightcolumnwidth}
        \textcolor{cvrulecolor}{\rule{\cvrightcolumnwidth}{0.3mm}}
    \end{minipage}

    % space after
    \vspace{\cvaftersectionskipamount}
}

% creates a standard, multi-purpose CV item with the given left and right column contents, parskip set to cvparskip
% in the right column, and with appropriate vertical space after
% @param #1 left column content
% @param #2 right column content
\newcommand{\cvitem}[2]{
    % left and right column
    \begin{minipage}[t]{\cvleftcolumnwidth}
        \raggedleft #1
    \end{minipage}% XXX necessary comment to avoid unwanted space
    \hspace{\cvcolumngapwidth}% XXX necessary comment to avoid unwanted space
    \begin{minipage}[t]{\cvrightcolumnwidth}
        \setlength{\parskip}{\cvparskip} #2
    \end{minipage}

    % space after
    \vspace{\cvafteritemskipamount}
}

% typesets a title, with appropriate vertical space after
% @param #1 title text
\newcommand{\cvtitle}[1]{
    % title
    \cvtitlestyle{#1}

    % space after
    \vspace{\cvaftertitleskipamount}
    % XXX need to subtract cvparskip here, because it is automatically inserted after the title "paragraph"
    \vspace{-\cvparskip}
}


% header and footer
% -----------------

% set empty header and footer
\pagestyle{empty}



% preamble end/document start
% ===========================

\begin{document}


% personal info
% -------------

\cvpersonalinfo{
    % photo
    \includegraphics[height=36mm]{nayan.jpg}
}{
    % name
    \cvname{Md.Masud Rana}

    % address
    \cvpersonalinfolinewithicon{height=4mm}{072-location.pdf}{
        Islampur, Bogura Sadar
    }

    % phone number
    \cvpersonalinfolinewithicon{height=4mm}{067-phone.pdf}{
        +88 01727\,218828
    }

    % email address
    \cvpersonalinfolinewithicon{height=4mm}{070-envelop.pdf}{
        nayan.fullstack@gmail.com
    }

    % LinkedIn account
    \cvpersonalinfolinewithicon{height=4mm}{458-linkedin.pdf}{
        masud-rana-89711591
    }

    % date of birth
    Born 21 March 1999
}


% work experience
% ---------------

\cvsection{WORK EXPERIENCE}

% Fake Company 2
\cvitem{
    \cvdurationstyle{May 2021 -- present}
}{
    \cvtitle{Full Stack Developer}

    Codebonds Limited, Bogura

    \begin{itemize}[leftmargin=*]
        \item Model–view–controller ( Our company follow mvc pattern
        to develop a software or a website )
        \item  Model (To create model we use C sharp )
        \item  VIEW (To create view we use HTML,CSS, Bootstarp )
        \item  Controller (To create controller we use Vue JS)
    \end{itemize}
}

% Fake Company 1
\cvitem{
    \cvdurationstyle{January  2021 -- April 2021}
}{
    \cvtitle{Internship}

    Codebonds Limited, Bogura

    \begin{itemize}[leftmargin=*]
        \item NGO Website ( In my internship period I'm working on a generation breakthrough project which based on online healthcare services)
        
        \item NGO Website details ( It has been design using HTML, CSS,  Bootstrap, JS, Jquery, MYSQL database and developing through PHP Laravel Framwork both for the admin panel and customer side )
    \end{itemize}
}


% education
% ---------

\cvsection{EDUCATION}


% bachelor's
\cvitem{
    \cvdurationstyle{2017 -- 2021}
}{
    \cvtitle{Bachelors's degree in CSE}

    The school of science and engineering, Southeast University

    \begin{itemize}[leftmargin=*]
        \item CGP: 3.16 
             
        \item Expected May 2021 
    \end{itemize}
}


% skills
% ------

\cvsection{SKILLS}

\vspace{\cvbetweensectionandheadingextraskipamount}

% languages
\cvitem{
    \cvheadingstyle{Languages}
}{
   C , C sharp , C++ , Java , JavaScript , Python , HTML , CSS , Bootstarp , XML, ASP , PHP 
}
\cvitem{
    \cvheadingstyle{Tools}
}{
    Visual studio , Microsoft SQL Management Studio , Android Studio , Sublime Test , XAMPP , Net beans ,PyCharm 
    

}

% fake skills
\cvitem{
    \cvheadingstyle{SOFT SKILL}
}{
    Communication , Teamwork , Flexibility , Confidence , Problem Solving , Self Management 
}

% completely fake skills
\cvitem{
    \cvheadingstyle{Additional Skill}
}{
    Database Management
    \begin{itemize}
        \item MYSQL, SQL server
    \end{itemize}

    Cloud-Based Technologies 
    \begin{itemize}
        \item AWS , Docker
    \end{itemize}
   
  
    }

% project
% ---------

\cvsection{PROJECT}


% Academic
\cvitem{
    \cvheadingstyle{Academic Project}
}{
    
    \begin{itemize}[leftmargin=*]
        \item Home automation using respberry pi ,DBMS project using MySql for Ticket Management system, Matlab project to find out area of a circle after detection, Smart Mobile Application for Southeast University , School Management System Website
             
    \end{itemize}
}

% Internship
\cvitem{
    \cvheadingstyle{Internship Project}
}{
    
    \begin{itemize}[leftmargin=*]
        \item  Devloping NGO Website ( Teamwork )
        
             
    \end{itemize}
}

% Academic
\cvitem{
    \cvheadingstyle{Job Project}
}{
    
    \begin{itemize}[leftmargin=*]
        \item IZMKS Website ( http://izmksweb.codebonds.com/ )
        
      
        
             
    \end{itemize}
}

% additional info
% ---------------

\cvsection{ADDITIONAL INFORMATION}

\vspace{\cvbetweensectionandheadingextraskipamount}



% interests
\cvitem{
    \cvheadingstyle{Interests}
}{
    Artificial Intelligence , Robotics , Travelling , Video Game
}

\end{document}
