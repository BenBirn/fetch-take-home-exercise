# Install and Run Instructions
- Download the file `main.py`
- Ensure you have a yaml file with the endpoints you want to test in the proper format
- In any terminal, navigate to the directory `main.py` is in, then run the command `python3 main.py <config_file_path>`

# Changes Made
## Ensure file is yaml file
```
if not config_file.endswith(".yaml"):
    print("Usage: <config_file_path> must be a yaml file")
    sys.exit(1)
```
Added to ensure the inputted file is a yaml file as opening another file type could have various unintended consequences in the code. Since the instructions say to assume any inputted yaml file is valid, no further file checking is needed.

## Precise time spent each loop
```
start = time.time()
...
while (time.time() < 15 + start):
    time.sleep(0.1)
```
Altered so that each loop takes 15 seconds as opposed to waiting 15 seconds after logging the results. Found by noticing the instructions state the check cycles should be 15 seconds "regardless of the number of endpoints or response times," which the original code didn't take into account

## Ignore port numbers for domain
```
domain = endpoint["url"].split("//")[-1].split("/")[0].split(":")[0]
```
Altered so that the domain ignores everything after the ":" if one exists, which can only be port name, as stated in instructions. Dound by noticing the instruction to ignore port numbers and googling how port numbers work in http addresses.

## Empty method consideration
```
method = endpoint.get('method', "GET")
```
Altered so that empty methods return `"GET"` as stated in the instructions instead of `None`. Found by reviwing the error that occured when running the code without this change.

## Convert body string to JSON
```
if body:
    body = json.loads(body)
```
Added to convert body string to JSON if it exists as that's what the requests function its used in expects. Only does this if body exists since `json.loads()` doesn't accept `None`. Found by noticing that the availability result was 25% when, according to the names in the sample yaml file, it should've been 50%.

## Response timeout
```
response = requests.request(method, url, headers=headers, json=body, timeout=0.5)
```
Altered response code to add the 500ms timeout as stated in the instructions. Found by reading the instructions and noticing the code didn't account for the timeout instruction.