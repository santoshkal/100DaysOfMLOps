# Task: Serve an ML Model with FastAPI

The xFusionCorp Industries ML platform team exposes the `fraud-detection` model through a FastAPI service and uses the auto-generated Swagger UI at `/docs` as the primary interactive surface—engineers submit predictions directly from the browser rather than crafting curl invocations. The FastAPI server is already running on port `8085` against the pre-trained model, and every prediction it serves is recorded in an in-memory audit log. Your task is to submit three distinct fraud-detection predictions through the Swagger UI's `Try it out` panel and confirm the server recorded every one.


1. The FastAPI server is already running on port `8085`. The FastAPI Swagger UI button at the top of the lab opens `/docs` — the interactive Swagger surface.

2. The project layout under `/root/code/serving/`:

  - `model.pkl` – Deterministic RandomForest trained at startup on the shared `amount / hour / num_tx_past_day → is_fraud` synthetic dataset.
  - `train.csv` – The 10-row source used to fit the model.
  - `app.py` – FastAPI application wired with `/health`, `POST /predict`, and `GET /last-predictions`. Correct and must remain intact — this lab does not edit the code.

3. Open the FastAPI Swagger UI button, expand the `POST /predict` section, click `Try it out`, enter a payload in the editable JSON panel, click `Execute`, and observe the response. Repeat three times with three distinct (`amount`, `hour`, `num_tx_past_day`) combinations.

4. The end state must include:

  - The Swagger UI at `/docs` is reachable (the FastAPI Swagger UI button loads it).
  - `GET http://localhost:8085/last-predictions` returns a JSON object whose count is at least 3 and whose predictions array carries at least three distinct (`amount`, `hour`, `num_tx_past_day`) tuples.
  - Every recorded prediction carries an `is_fraud` flag of `0` or `1`.

> Suggested payloads to exercise both classes: `{"amount": 3200, "hour": 23, "num_tx_past_day": 5}` (high-value, late-night—expected to flag fraud); `{"amount": 25.5, "hour": 10, "num_tx_past_day": 1}` (low-value, daytime—expected to pass); `{"amount": 890, "hour": 2, "num_tx_past_day": 3}` (mid-range borderline).

---

# Solution:

- This is about invoking the `/predict` endpoint from the Swagger UI with provided three distinct fraud-detection prediction payloads.

- cd into the `serving` directory, and inspect the `./app.py`. Per description, the server is functioning and should run fine. Start the server with
`python3 ./app.py &`

![run-app.py](./assets/mlops-day58.png)

- Open the Swagger UI by clicking the button on top-right of the lab terminal.

![open-swagger-ui](./assets/mlops-day58a.png)

- Check `/health` endpoint to see if everything is working:

![check-health](./assets/mlops-day58b.png)

- Now, we invoke the `/predict` endpoint each with the following parameters
  - 
  - `{"amount": 3200, "hour": 23, "num_tx_past_day": 5}` 
  - `{"amount": 25.5, "hour": 10, "num_tx_past_day": 1}` 
  - `{"amount": 890, "hour": 2, "num_tx_past_day": 3}` 

Click **Try it out** and update the values accrdingly, and hiit execute for all three predictions. These should return a different `is_fraud` value for each prediction.

![check-predcit](./assets/mlops-day58c.png)

- Now on the lab terminal, we need to query the `/last-predictions` endpoint which should return the same three predictions in an array with same values we
provided on the Swagger UI.

```
curl http://localhost:8085/last-predictions
```

![last-rediction](./assets/mlops-day58d.png)

Hit **Check**


