<!DOCTYPE html>
<html>
<head>
    <title>Index Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        ul {
            list-style-type: none;
            padding-left: 0;
        }
        li {
            display: flex;
        }
        li::before {
            content: counters(section, ".") " ";
            counter-increment: section;
            flex: 0 0 50px; /* Set a fixed width for alignment */
        }
        ul ul {
            counter-reset: section;
            padding-left: 0;
        }
    </style>
</head>
<body>

<h1>Index</h1>
<ul>
    <li>
        <span>Section 1</span>
        <ul>
            <li>
                <span>Subsection 1.1</span>
                <ul>
                    <li>
                        <span>Subsubsection 1.1.1</span>
                    </li>
                    <li>
                        <span>Subsubsection 1.1.2</span>
                    </li>
                </ul>
            </li>
            <li>
                <span>Subsection 1.2</span>
                <ul>
                    <li>
                        <span>Subsubsection 1.2.1</span>
                    </li>
                </ul>
            </li>
        </ul>
    </li>
    <li>
        <span>Section 2</span>
        <ul>
            <li>
                <span>Subsection 2.1</span>
            </li>
        </ul>
    </li>
</ul>

</body>
</html>
