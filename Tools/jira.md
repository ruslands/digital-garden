(resolution = Unresolved and status != Closed) and (reporter = currentUser() or assignee = currentUser()) order by created DESC

(resolution = Unresolved and status != Closed) and (reporter = currentUser() or assignee = currentUser()) order by created DESC

project = 10301 AND fixVersion in (10933, 10906, 10872, 10859, 10853) AND (component = CRM OR component  = Communication) AND  component  != Backend  ORDER BY assignee ASC, priority DESC, key ASC

project = 10301 AND fixVersion in (10933, 10906, 10872, 10859, 10853) AND component = CRM OR component  = Communication ORDER BY assignee ASC, priority DESC, key ASC

project = 10301 AND fixVersion in (10933, 10906, 10872, 10859, 10853) AND component = CRM AND component  = Communication AND component  != Backend  ORDER BY assignee ASC, priority DESC, key ASC

status not in (Production, "Ready for release", "Won't do", Done, Closed, Resolved) AND (reporter = currentUser()  OR assignee = currentUser()) ORDER BY status DESC

# release notes

issuetype != Epic AND status in (Production, "Ready for release") AND component = Communication ORDER BY status DESC