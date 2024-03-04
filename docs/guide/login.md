# __Přihlašování přes STAG__

- [Dokumentace](https://is-stag.zcu.cz/napoveda/web-services/ws_prihlasovani.html)

```python
if "stagUserTicket" not in st.session_state:
    ticket = st.experimental_get_query_params().get("stagUserTicket")
    st.session_state["stagUserTicket"] = ticket

if not st.session_state["stagUserTicket"] or st.session_state["stagUserTicket"][0] == "anonymous":
    idnos = None
    login_url = os.getenv("LOGIN_URL")
    st.header(f"Přihlašte se pomocí [STAGu]({login_url})")
else:
    # zbytek aplikace
    ...
```
