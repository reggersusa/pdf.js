# pdf.js
PDF Reader in JavaScript
<p>The point of the this repo is to support pdf.worker.js being hosted on another domain.
I am proposing a change to src/api.js which allows this support.</p>
<em>Problem descritpion:</em>
<p>Our end-user product is delived with the binaries (DLLs and etc.) on the host domain
and the static files (JS, CSS, PNG and etc.) on another domain. When trying to set thePDFJS.workerSrc
with the path of that domain, a cross origin execution exception is thrown on the call to new Worker().</p>
<em>Proposed solution:</em>
<p>To get around this, I adapted the WorkerTransport initialization to test whether PDFJS.workerSrc is a function.
If not, it behaves normally. If it is a function, it calls the function passing a callback,
expecting to receive the JS code of the pdf.worker.js itself. From this, it creates a blob
using PDFJS.createObjectURL(), which avoids the cross-origin exception. The host application is responsible
for fetching the worker source code and returning the JS code.</p>
